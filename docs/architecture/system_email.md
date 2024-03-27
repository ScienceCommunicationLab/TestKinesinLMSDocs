# System Email

( Content to come...)

## Survey Reminder Emails

( Content to come...)

## Automating Survey Reminder Emails

There's simple management command that will send out any current survey reminder emails.

Ideally this command would be run once a day, but it could be run more or less frequently if desired.

One way to run it daily is to set up a chron command to run the management command.

To run the task manually: ::

     heroku run python manage.py survey_emails --remote production

On heroku, you can use the use Heroku Advanced Scheduler to run the task each day.

Setting this up is outside the scope of these docs. You can find more
information [here](https://devcenter.heroku.com/articles/scheduler).

Once you have an advanced scheduler set up, you can view the automated task's logs: ::

    heroku logs -t -a (your app name) -d advanced-scheduler




