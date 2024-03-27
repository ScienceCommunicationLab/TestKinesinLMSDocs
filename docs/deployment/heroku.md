# Heroku Deployment

## Background

KinesinLMS is a Django app that can be hosted on any Platform-as-a-Service (PaaS) that supports Django, such as Heroku,
Fly.io or Render.com. (It can also be hosted on your own custom-built server or platform!)

These instructions will describe setting up on Heroku, but the basic steps are the same for any PaaS.

## Heroku Pipelines

Typically, there are three servers organized into a Heroku pipline:

* development -- for testing and debugging new features
* staging -- for team testing and validation of production-ready builds
* production -- the live server

You can set the environment variable `HEROKU_PIPELINE` to explicitly declare which one you're on, so that KinesinLMS is
made aware of which of these servers its running on and modify code accordingly. (E.g. when not running on
the `PRODUCTION` pipeline, maybe only send emails to the developer.)

## Heroku Setup

In this step will set up the Heroku apps for the three parts of our pipeline.
We assume you have a Heroku account and have installed the Heroku CLI.

Note that these instructions are based on Heroku set up steps provided by Django Cookiecutter.
You can read those instructions here: https://cookiecutter-django.readthedocs.io/en/latest/deployment-on-heroku.html

Create the Heroku apps for each step in our pipeline. Use the name of your app in place of `(your app name)` in the
following

    heroku create --buildpack heroku/python --remote development --app (your app name)-development

    heroku create --buildpack heroku/python --remote staging --app (your app name)-staging

    heroku create --buildpack heroku/python --remote production --app (your app name)

Set up each app in the pipeline. The following instructions are for the production app.

## PostgreSQL Setup

KinesinLMS uses PostgreSQL as its database. We'll use the Heroku PostgreSQL add-on.

Create a PostgreSQL database: ::

    heroku addons:create heroku-postgresql:mini --remote production

Schedule backups (since the 'mini' plan doesn't include automatic backups): ::

    heroku pg:backups schedule --at '02:00 America/Los_Angeles' DATABASE_URL --remote production

...and then 'promote' the database to be our primary database: ::

    heroku pg:promote DATABASE_URL --remote production

## REDIS Setup

Parts of KinesinLMS use Redis for caching and other purposes, such as asynchronous tasks run by Celery.

We'll use the Heroku Redis add-on: ::

    heroku addons:create heroku-redis:mini --remote production

    heroku config:set CELERY_BROKER_URL=$(heroku config:get REDIS_TLS_URL) --remote production

## Environment Veriables Setup

Now we set up some various environment variables that Django expects, as well as some custom ones we've
created to configure our application. ::

    heroku config:set PYTHONHASHSEED=random  --remote production
    heroku config:set WEB_CONCURRENCY=4  --remote production
    heroku config:set DJANGO_DEBUG=False  --remote production
    heroku config:set DJANGO_SETTINGS_MODULE=config.settings.production  --remote production
    heroku config:set DJANGO_SECRET_KEY="$(openssl rand -base64 64)"  --remote production
    heroku config:set DJANGO_ADMIN_URL="$(openssl rand -base64 4096 | tr -dc 'A-HJ-NP-Za-km-z2-9' | head -c 32)/" --remote production

    heroku config:set DJANGO_ALLOWED_HOSTS=kinesinlms.herokuapp.com  --remote production

If you want to use Google Analytics, set the following environment variable to your google analytics ID : ::

    heroku config:set DJANGO_GOOGLE_ANALYTICS_ID=XXXXXXX  --remote production

## AWS Access Setup

KinesinLMS expects to place static and media files in an AWS S3 bucket. So you'll need to create a bucket
on AWS S3 to hold these assets, as well as an AWS IAM user with access to that bucket. Describing how to
create these in AWS is outside the scope of this document, but you can read more about it here:
https://simpleisbetterthancomplex.com/tutorial/2017/08/01/how-to-setup-amazon-s3-in-a-django-project.html

Once you've created an S3 bucket for the KinesinLMS app and an IAM user with access to that bucket,
create an access key and secret for that IAM user.

The use those values to set the following environment values in the Heroku environment ::

    heroku config:set DJANGO_AWS_ACCESS_KEY_ID=(your aws access key) --remote production
    heroku config:set DJANGO_AWS_SECRET_ACCESS_KEY=(some django aws secret access key) --remote production
    heroku config:set DJANGO_AWS_S3_REGION_NAME=us-west-1 --remote production

## Custom Environment Variables Setup

There are a few custom environment variables that KinesinLMS uses to configure the application.
We'll set those now:

    heroku config:set DJANGO_PIPELINE="PRODUCTION" --remote production

## Pre-Deploy Steps

You're now ready to push the app to heroku. But before you do, make sure you've compiled your
stylesheets, packaged your javascript, and basically got everything ready to go.

### Compile Stylesheets

If there has been any changes to scss files, make sure they're compiled to css files.

In an effort to keep things simple, there's an npm package.json command to watch scss files
directly using sass. (You'll need sass installed on your machine.)

To run sass in watch mode the 'watch-styles' npm command: ::

    npm run watch-styles

When this command is run, all .sass files in the kinesinlms/static/sass directory will be watched
and compiled to .css files in the kinesinlms/static/css directory.

## Package Javascript

Make sure all javascript-based components have been transpiled and packaged.
(You'll need node and npm installed on your machine.)
Note that this part of the build step is somewhat rudimentary and will be
improved in the future.

If you're pushing to production that means building minified javascript files: ::

    npm run build

But if you want to debug on DEVELOPMENT or STAGING, create javascript with source maps ::

    npm run dev

This will create compiled javascript files in kinesinlms/static/js.

IMPORTANT: Make sure you add any new or updated files to github before continuing.

## Deploy to Heroku

IMPORTANT: Make sure you add any new or updated files from the previous steps to your git repository before continuing,
otherwise they won't get pushed to Heroku in the next step.

DOUBLY-IMPORTANT: Make sure all your tests are passing before pushing to production.

Pushing your project to Heroku is as simple as pushing to github. ::

    git push production master

## Post-deploy

If you're setting up a server for the first time (or just cleaned out DEVELOPMENT and
want to set up initial data), run the following management command to set up all initial data.

For example if you're setting up DEVELOPMENT ::

    heroku run python manage.py setup_all --remote development

Then set up a superuser so that you can log in and get to work on the Django admin screen:

    heroku run python manage.py createsuperuser --remote development
