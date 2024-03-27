# Badge Provider Integration

In order to award badges, you'll need to integrate with a third-party badge service. We've chosen to create an
integration with Canvase Badgr, which is a free service that provides a simple API for awarding badges.
The `BadgrBadgeService` class implements the Badgr API such that Badges can be awarded upon certain events, like
passing a course.

If you want to use a different service, you'll need to create a new subclass of
the `BadgeService` class to implement the new service, and potentailly update or subclass `BadgeProvider` to match.
You'll also need to add a new name and value pair to `BadgeProviderType` enum to represent your integration.

## Setting Up an Account at Badgr.com

You can create a free accont at Badgr.com here: [https://badgr.com/auth/signup](https://badgr.com/auth/signup)

After you do, use the Badgr.com admin website to create an "issuer" for your KinesinLMS site.

You'll need the issuer "ID" in the next step. To get it, go to the "public" page for the issuer and then click
"View JSON" button at the bottom of the page. This will show you the JSON for the issuer, and the "id" field is the
full path for the issuer ID. It will look something like this: `https://api.badgr.io/public/issuers/(some long string)`

That final long string is the issuer "ID" and the sting you'll need in the next step. So copy it and hold on to it for
the next step.

## Setting up the Badge Provider

Once you've created a new Badgr.com account and an issuer, you can set up your Badge Provider in the KinesinLMS admin
panel.

The first step is to set the two environment variables for your login credentials:
BADGE_PROVIDER_USERNAME=(your username)
BADGE_PROVIDER_PASSWORD=(your password)

Then create a Badge Provider in the KinesinLMS management panel. Here's how:

1. Go to the Badge Providers admin page: `https://(your app url)/management/badge_provider/`
2. Add "Badgr" as the name of the provider.
3. Set the "type" to "Badgr".
4. Create a slug for this provider. `badgr` is a good choice.
5. Set the "api url" to `https://api.badgr.io/`
6. Create a random "salt" string.
7. Add the issuer ID from the last step in the "Issuer entity ID" field

You can leave the "Access token" and "Refresh token" fields blank for now. They will be filled in automatically
when the badge provider first accesses the Badgr API.

## Creating Badge Classes

Now that you have a connection with Badgr, you can set up what are called "badge classes" to represent achievements.
Badge classes are represented in KinesinLMS with the `BadgeClass` model.

Read the [Creating a Badge Class for Course Completion](creating_badge_class.md) documentation for more information on
how to set up a badge class for course completion.





