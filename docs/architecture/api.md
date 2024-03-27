# KinesinLMS API

## Overview

The KinesinLMS API is a RESTful API implemented using Django Rest Framework (DRF).

There are two basic sets of API endpoints for kinesinlms.

1. "Internal" endpoints: The first is for components within KinesinLMS that need to access the site via API, such as
   "Simple Interactive Tool" components, which load their initial data and submit student activity via API.
2. "Analytics" endpoints: The second is for the external applications that need analytics data.

Both sets of endpoints are configured in the top-level `config/urls.py` module.

## Internal Endpoints

At the moment, the main use of "internal" API endpoints are the React-based video component and
simple interactive tools that we load in a course unit page.

Internal endpoints are configured in the Router under `router = routers.DefaultRouter()`

A call to an endpoint here starts with `/api/`

## Analytics Endpoints

Analytics endpoints are meant to be called by an external resource -- the kinesinlms-analytics app -- and therefore use
the default token-based authentication provided by DRF.

A call to an endpoint in this group starts with `/api/analytics/`
