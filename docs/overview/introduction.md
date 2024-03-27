# Introduction

KinesinLMS is a Learning Management System (LMS) designed to be a simple, easy-to-use, and flexible platform for
managing and delivering online courses. It's a 'just enough' LMS that you can use as a base for your e-Learning
experiments and research projects.

KinesinLMS is built on the Django web framework and uses standard open-source tools and libraries wherever
possible:

- [PostgresSQL](https://www.postgresql.org/) for the database.
- [Bootstrap](https://getbootstrap.com/) for styles.
- [Django Rest Framework](https://www.django-rest-framework.org/) for the API.
- [Celery](https://docs.celeryq.dev/) for asynchronous tasks.

...and so on.

For more complex things like badges, email automations, surveys and forums, KinesinLMS expects you'll use third-party
services and therefore provides simple integrations for each...although there's nothing stopping you from creating your
own custom implementations of these features.

# Hypermedia and "Rich" Interactions

KinesinLMS is opinionated about "rich" clients: it assumes that for most user interactions you can
use ["hypermedia"](https://hypermedia.systems/) via HTMx to get immediate and engaging interactions without
introducing a complicated front-end framework. Furthermore, using HTMx allows the developer to continue to
use the simple (and effective) Django form classes rather than crafting custom APIs for every interaction.

However, certain features like fully interactive assessments may require a more complex
front-end. In that case we use React. But we try to keep the React components as simple as possible, and continually
re-evaluate whether we need React and its build system at all. (Web components are another option we're considering to
further reduce complexity.)

We try to remember that the benefit of each "rich" interaction added to the platform must be balanced against the
complexity it introduces, potentially increasing the number of developers who need to work on the project and decreasing
the number of students who can access the course through their (potentially limited) mobile devices.

## Background

KinesinLMS originated as a custom e-Learning platform developed
by [McQuillen Interactive](https://www.mcquilleninterative.com)
for [Science Communication Lab (SCL)](https://www.sciencecommunicationlab.org/) as part of SCL's iBiology Courses project.
SCL funded the work with support from the National Institute for General Medical Sciences (grants #5R25GM116704 and #1R25GM139147).

[iBiology Courses](https://courses.ibiology.org) is an e-learning site dedicated to helping university students and
post-graduate researchers become better scientists and enhance career and professional development. The iBiology Courses
e-Learning platform was built from scratch after the team struggled to find a simple, agile and easily extensible
platform for SCL's unique e-learning research goals.

In 2023, SCL was funded by a supplementary NIH grant to make this custom e-Learning platform open source and available
to the broader scientific community. KinesinLMS is the result of that effort!


