# The Jirafe Events API

In the documentation, some URIs contain the symbols “{app-id}” and “{site-id}.”
These symbols should be replaced with your *Application Identifier* and your *Site Identifier*, which are available from Jirafe support.

Notes:
* Currently all fields are required fields.
* All dates are given in ISO 8601:2004 format.
* All times are given in UTC.

## Endpoints
The Jirafe Events API exposes the following URI endpoints:
* http://events.jirafe.com/v1/{app-id}/{site-id}/category [POST]
* http://events.jirafe.com/v1/{app-id}/{site-id}/product [POST]
* http://events.jirafe.com/v1/{app-id}/{site-id}/cart [POST]
* http://events.jirafe.com/v1/{app-id}/{site-id}/order [POST]

### The Category Endpoint
http://events.jirafe.com/v1/{app-id}/{site-id}/category [POST]

The category endpoint accepts JSON objects with the following format:
```json
{
    "id": "",
    "name": "",
    "change_date": "YYYY-MM-DD:H-M-S UTC",
    "create_date": "YYYY-MM-DD:H-M-S UTC"
}
```

#### Example
```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d ‘{“id”:”43110”, “name”:”World”}’ \
       http://events.jirafe.api/v1/{site-id}/{app-id}/category
```

Should return:
```
200 OK
```




### The Product Endpoint
http://events.jirafe.com/v1/{app-id}/{site-id}/product [POST]

The product endpoint accepts JSON objects with the following format:
```json
{
    "id": "",
    "sku": "",
    "name": "",
    "cost": 0.0,
    "price": 0.0,
    "inventory": 0,
    "rating": 0,
    "brand": "",
    "change_date": "YYYY-MM-DD:H-M-S UTC",
    "create_date": "YYYY-MM-DD:H-M-S UTC",
    "attributes": [
        {
            "id": "",
            "name": "",
            "value": ""
        },
    ],
    "categories": [
        {
            "id": "",
            "name": ""
        },
    ],
    "images": [
        {
            "url": ""
        },
    ],
    "urls": {
        "store": "",
        "admin": ""
    }
}
```

### The Cart Endpoint
http://events.jirafe.com/v1/{app-id}/{site-id}/cart [POST]
```json
{
    "id": "",
    "customer_id": "",
    "change_date": "YYYY-MM-DD:H-M-S UTC",
    "create_date": "YYYY-MM-DD:H-M-S UTC",
    "subtotal": 0.00,
    "total": 0.00,
    "promo": {
        "amount": 0.00,
        "type": "",
        "code": ""
    },
    "items": [
        {
            "id": "?",
            "product": {},
            "quantity": 0,
            "change_date": "YYYY-MM-DD:H-M-S UTC",
            "promo": {
                "amount": 0.00,
                "type": "",
                "code": ""
            }
        }
    ]
}
```

### The Order Endpoint
http://events.jirafe.com/v1/{app-id}/{site-id}/order [POST]

The order endpoint accepts JSON objects with the following format:
```json
{
    "id": "",
    "customer": {
"id",
"name": "",
    },
    "cart_id": "",
    "order_date": "YYYY-MM-DD:H-M-S UTC",
    "change_date": "YYYY-MM-DD:H-M-S UTC",
    "create_date": "YYYY-MM-DD:H-M-S UTC",
    "subtotal": 0.00,
    "total": 0.00,
    "promo": {
        "amount": 0.00,
        "type": "",
        "code": ""
    },
    "items": [
        {
            "id": "",
            "product": {},
            "quantity": 0,
            "promo": {
                "amount": 0.00,
                "type": "",
                "code": ""
            }
        }
    ]
}
```
