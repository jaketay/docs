---

layout: page
title: Authentication
categories: API Documentation
resource: true
description: Authenticating with Jirafe API End Points
image: /img/docs-api.png
version: v2
order: 2
index_order: 2
index_title: API Documentation

---


# Authentication
In order to do anything with Jirafe API, you must be able to authenticate with our platform.  In order to do so, you need:

* Site ID and auth credentials for your development environment
* Site ID and auth credentials for your production environment
* For each site you want to track seperately with Jirafe, you will need a distince `{site-id}` and authentication token.
* You will also need a `{client_id}` and `{client-secret}`. You will need a distinct set for each client your going to be authenticating with Jirafe.  In the case you are a SaaS platform, you most likely will only need one.

You can get a `{site-id}` and authorization credentials by make contacting Jirafe Support and applying for access.

In all of the requests to the Jirafe API, it is assumed that this token will be sent as a bearer token in the `Authorization` header.


## Example
For example, if you are given an oauth-token of **45dc86d2e2fc4342939a2b6791cfcb9b1c4b89a0**, then in all requests you make to the event api you will include this header:

```
...
Authorization: Bearer 45dc86d2e2fc4342939a2b6791cfcb9b1c4b89a0
...
```

In some of this documentation's example code sections, the fake oauth token given above is used.
Please replace this fake token with your own when running the example code.