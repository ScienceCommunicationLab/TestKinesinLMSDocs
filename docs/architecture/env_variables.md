# Environment Variables

Most configuration information is stored in the KinesinLMS database. However, some information that is secret
and shouldn't be stored in the DB, is by convention stored in the environment alongside the usual Django settings.

The following environment variables should be configured if the user wants to use various services.

For an example of how to set these variables, see the `env.example` file in the root of the project.

## Email Service

To send email directly, Django must be configured to use an email gateway.
IMPORTANT: Along with setting the following environment variable. The correct variant of the AnyMail library must
be set in production.txt, and AnyMail configured in production.py

EMAIL_SERVICE_TOKEN="(some API key from email service)"

## Email Automation Provider

If an email automation provider is being used, most information will be stored in the EmailAutomationProvider model.
However, the following should be set and stored in the environment:

EMAIL_AUTOMATION_PROVIDER_API_KEY="(some API key from email automation service)"

## Survey Provider

If a survey provider is being used, most information will be stored in the SurveyProvider model.
However, the following should be set and stored in the environment:

SURVEY_PROVIDER_API_KEY="(some api key from survey provider)"

## Forum Provider

If a forum provider is being used, most information will be stored in the ForumProvider model.
However, the following should be set and stored in the environment:

FORUM_API_KEY="(some API key from forum provider service)"
FORUM_SSO_SECRET="(some SSO secret from forum provider service)"
FORUM_WEBHOOK_SECRET="(some webhook secret from forum provider service)"

## Badge Provider

If a badge provider is being used, most information will be stored in the BadgeProvider model.
However, the following should be set and stored in the environment:

BADGE_PROVIDER_USERNAME="(some username from badge provider service)"
BADGE_PROVIDER_PASSWORD="(some password from badge provider service)"

## Other Integrations

If you use Sentry (highly recommended), set the DSN for your project in the SENTRY_DSN environment variable.

SENTRY_DSN="(some DSN from Sentry)"

If you have recaptcha set up with google, you can configure it with the following:

RECAPTCHA_USE_RECAPTCHA=False
RECAPTCHA_PUBLIC_KEY=(some public key)
RECAPTCHA_PRIVATE_KEY=(some private key)

## Other Env Variables...

For more infomation on other standard environment variables to configure Django and Celery, see the .env.example
file in the root of the project.
