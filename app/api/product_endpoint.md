---
permalink: /v2/POST/product_endpoint.html
layout: page
title: Product Endpoint
categories: API Documentation
resource: true
version: v2
---

# Product Endpoint
**URL:** https://event.jirafe.com/v2/{site-id}/product [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/event-api/jsonschema/v2/product.json

#### Example
```json
{
    "id": "8796107014145",
    "create_date": "2013-03-28T19:46:39.000Z",
    "change_date": "2013-03-28T19:50:58.000Z",
    "is_product": true,
    "is_sku": true,
    "catalog": {
        "id": "electronicsProductCatalog",
        "name": "Electronics Product Catalog"
    },
    "name": "PowerShot A480",
    "code": "1934793",
    "brand": "Canon",
    "categories": [
        {
            "id": "8796098461838",
            "name": "Digital Compacts"
        },
        {
            "id": "8796099248270",
            "name": "Canon"
        }
    ],
    "images": [
        {
            "url": "http://yourstore.com/images/the_photo.jpg"
        }
    ]
}
```
