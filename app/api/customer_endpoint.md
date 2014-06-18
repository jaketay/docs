---

layout: page
title: Customer Endpoint
categories: API Documentation
resource: true
description: Sending Customer Data to Jirafe API
version: v2
order: 6

---

# Customer Endpoint
**URL:** https://event.jirafe.com/v2/{site-id}/customer [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/customer.json

**Interactive API Tester:** http://docs.jirafe.com/api/event/#!/customer

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
