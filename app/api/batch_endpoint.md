---

layout: page
title: Batch Endpoint
categories: API Documentation
resource: true
description: Sending data to Jirafe API in batch
version: v2
order: 3

---

# Batch Endpoint
**URL:** https://event.jirafe.com/v2/{site-id}/batch [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/batch.json

This endpoint allows for up to 10MB of events to be submitted in a single request. The object is formed of arrays of the individual endpoints that would normally be sent.

**Response:** The response will be a matching object of arrays containing the individual responses for each call as if it was possed to an individual endpoint.

#### Example
```json
{
    "category": [
        {
            "id": "8796094234766",
            "name": "Category 1",
            "change_date": "2013-03-28T19:44:11.000Z",
            "create_date": "2013-03-28T19:38:09.000Z"
        },
        {
            "id": "8796097654766",
            "name": "Category 2",
            "change_date": "2013-03-28T19:44:11.000Z",
            "create_date": "2013-03-28T19:38:09.000Z"
        }
    ],
    "employee": [
        {
            "id": "532sdfg",
            "active_flag": true,
            "change_date": "2013-03-28T19:38:03.000Z",
            "create_date": "2013-03-28T19:38:03.000Z",
            "first_name": "Product",
            "last_name": "Manager",
            "email": "product_manager@example.com"
        }
    ]
}
```
