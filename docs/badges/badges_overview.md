# Badges Overview

"Open Badges" are digital images (usually .png or .svg format) that encode accomplishments in a standardized format.
Information about the accomplishment that earned that badge, including who earned it and the criteria involved,
is cryptographically signed and embedded in the image.

Using cryptography to encode this information allows the badge to be verified as
authentic and to be shared reliably across the web. Badges can be displayed on social media, in email,
on websites, and in digital portfolios. They can also be collected and displayed in a "backpack"
or "passport" system, such as Badgr.com, Mozilla Backpack, or Open Badge Passport.

The open badges architecture is governed by a published, evolving standard, currently
at [version 2.1](https://www.imsglobal.org/spec/ob/v2p1/).

The use of badges as a reward system in online courses has been shown to encourage user interaction with courses and
improve course completion rates.

KinesinLMS provides a "BadgeProvider" class to define a third-part badge provider. At the moment,
the only provider supported is for Badgr.com.

To award badges for things like course completion, you'll need to create an account on Badgr.com and then set up
BadgeProvider in KinesinLMS to represent that account.

Then, for every badge you want to award, you'll need to set up what's called a "badge class" in Badgr, and then copy
that information into a badge class on KinesinLMS.

At the moment, KinesinLMS only supports awarding badges for course completion. However, the architecture is
designed to be extensible, so it should be possible to add support for other types of badges in the future.

More resources on Open Badges:

- [What are Open Badges](https://community.canvaslms.com/t5/Canvas-Badges/What-are-Open-Badges/ta-p/528726)
- [Getting Started with Open Badges and Open Microcredentials](https://files.eric.ed.gov/fulltext/EJ1240709.pdf)
- [Open Badges](https://openspace.etf.europa.eu/pages/open-badges)


