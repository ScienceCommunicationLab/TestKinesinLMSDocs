# Email Automation Provider Integration

This section explains how to set up an email automation provider. At the moment, this process is
specific to ActiveCampaign, but it should be relatively easy to add support for other providers.

## ActiveCampaign Credentials

The first step to integrating with an email automation service is creating an account and getting credentials
required in order to use the API. If an email automation provdier is configured, KinesinLMS will use the service's API
to send events to the email automation provider as students interact with the system. To do this, you'll need to
obtain an API key and API URL from the provider.

It's easy to create an ActiveCampaign test account and obtain API credentials. ActiveCampaign
provides [free developer accounts for testing purposes](https://developers.activecampaign.com/page/getting-started).

## Setting Up an Email Automation Provider in KinesinLMS

Once you have your account, you can find your API credentials by logging into your account on ActiveCampaign and
navigating to the "Settings" page and clicking on the "Developer" tab.

You'll need both the "API URL" and the "API Key" shown on this page to configure KinesinLMS.

KinesinLMS tries not to store secret information in the database, but rather expects you to set
secrets as environment variables. This is a good practice for security reasons.

In this case, you'll need to set the following environment variable based on the API key you've obtained from
ActiveCampaign. If you're doing this in an .env file, you'll add a line in your file that looks like this:

```
EMAIL_AUTOMATION_PROVIDER_API_KEY=(your active campaign api key)
```

Then, after restarting and logging into KinesinLMS, go to the management > email automation provider page. This page
allows you to configure all other settings for your email automation provider connection.

On this page, you'll see a simple form for turning on (via the "Active" checkbox) and managing your email automation
provider connection. You'll need to set the "type" field to "ActiveCampaign" and the "API URL" to the value you obtained
from ActiveCampaign.

Note: Remember that this page doesn't provide a way to set the API key...you need to set the .env variable as explained
agove. However, thispage will warn if you haven't set it in the environment.

Save your changes, and you should be good to go! You can test your connection with the "Test Connection" button at the
bottom of the page.

You can safely ignore the Tag ids field. This field is used to store mappings from tags used in the email automation
provider to their integer IDs. Unfortunately, some providers (like ActiveCampaign) require a developer to send tag IDs
rather than just the tag itself during API calls. As it operates, KinesinLMS will automatically populate this field with
the tag IDs it needs to send events to the email automation provider.

But wait, you're not completely done yet. You still have to configure each course to send events to the email
automation.

## Setting Up Email Automation Provider Events in a Course

To configure a course to send events to the email automation provider, go to the course's settings page and click on the
"Email Automations" tab.

On this page, you can activate the connection with the "Active" checkbox.

You can also select which events you want to send to the email automation provider. At the moment, only
three events are available for selection:

- User registration
- "Course Enrolled"
- "Course Unenrolled"
