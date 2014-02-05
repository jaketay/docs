---
permalink: /v2/POST/customer_endpoint.html
layout: page
title: Customer Endpoint
categories: API Documentation
resource: true
version: v2
---

# Customer Endpoint
**URL:** https://event.jirafe.com/v2/{site-id}/customer [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/event-api/jsonschema/v2/customer.json

#### Example
```json
{
    "id": "1234abc",
    "active_flag": true,
    "change_date": "2013-06-17T15:15:53.000Z",
    "create_date": "2013-06-17T15:15:53.000Z",
    "email": "john.doe@gmail.com",
    "first_name": "John",
    "last_name": "Doe",
    "name": "John Doe"
}
```