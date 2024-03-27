#Local Setup

This page describes how to set up KinesinLMS on your local machine. It's biased towards Mac OS, but the steps are
similar for other operating systems.

# Virtual Environment

Create a virtualenv for the project. On MacOS, with mkvirtualenv installed, you can do this with the following command
(assuming your python3 is at `/usr/local/bin/python3`):

```bash
    mkvirtualenv --python=/usr/local/bin/python3 kinesinlms
```

If your environment isn't already activated, activate it:

```bash
workon kinesinlms
```

Next, install the project's python dependencies:

```bash
    pip install -r requirements/local.txt
```

After that, we'll install tools to help us build the project's javascript dependencies.

Although KinesinLMS tries to keep any javascript build steps to a minimum, we still need to build a few javascript
libraries from source definitions like those in `static/src/` and in `kinesinlms_components`. You will need to have npm
and node installed to build these javascript and typescript source files.

If you're on MacOS and you don't have those installed, you can install them with homebrew:

```bash
    brew install node
```

Then, install the project's javascript dependencies:

```bash
    npm install
```

And finally, build the client code:

```bash
    npm run build
```

This creates the bundled javascript files needed by the app (mainly for the advanced assessment tools like
DiagramTool and TableTool).

While you're working, you'll want the scss to compile to css automatically. You can do that with the following command:

```bash
    npm run watch-styles
```

## Set up environment variables

You'll need some basic enviroment variables set to run the project. These variables will looks slightly different
when you're running locally as compared to when you're running in development, staging or production or your PaaS.

When setting up to run locally, you can copy the example file `.env.example` to a file like `_envs/_local/django.env`,
and then change the values as you wish (you'll need to know a bit about how Django uses these variables).
Make sure to configure your IDE to use those variables when running the Django server, shell, or a management command.

If you're using PyCharm, you can use the EnvFile plugin in the
run configuration to read in the environment variables frmo a file.

## Database

For this next step, you'll need postgres set up on your machine and running. If you don't have it installed, you can
install it with homebrew:

```bash
    brew install postgres
```

Then, start the postgres server:

```bash
    brew services start postgres
```

Create a PostgreSQL database for our local development:

```bash
    psql -c "CREATE DATABASE kinesinlms;"
```

Then, migrate the database:

```bash
    python manage.py migrate
```

## Django Setup

You'll probably want a Django superuser to manage the system, so create one now:

```bash
    python manage.py createsuperuser
```

You'll also need to set up some initial model instances and do a few other configuration steps to get
the app ready. We've created a `setup_all` management command to do that. Feel free to extend that command
to do other initial setup tasks you require.

```bash
    python manage.py setup_all
```

## Redis Setup

KinesinLMS uses Redis for for managing background tasks. You'll need to have Redis installed and running
if you want to run these background tasks locally. Otherwise, you can set the `CELERY_TASK_ALWAYS_EAGER` setting
to `True` in your local environment to run the tasks synchronously.

If you don't have Redis installed, you can install it with homebrew:

```bash
    brew install redis
```

Then, start the Redis server:

```bash
    brew services start redis
```

You'll now need to run Celery as a separate task. You can do that with the following command in your project's
root directory. Make sure all the environment variables are set before running this...especially
`DJANGO_SETTINGS_MODULE=config.settings.local` ... just like when running Django.

```bash
    celery -A config worker -Q celery -l DEBUG
```

## Third-party Services

### Discourse Forums

If you're going to use Discourse as a forum service, you'll probably want to run Discourse, which isn't exactly easy,
but is relatively straightforward if you're familiar with Docker. You can run a docker image of Discourse and configure
it to look similar to your production environment.

View the "Local Setup" page in the "Forum" section for more information.
