# The Jirafe Events API

In the documentation, some URIs contain the symbols “{org-id}” and “{site-id}.”
These symbols should be replaced with your *Organization Identifier* and your *Site Identifier*, which are available from Jirafe support.

Notes:
* All dates are given in ISO 8601:2004 format.
* All dates and times are given in UTC.

## Endpoints
The Jirafe Events API exposes the following URI endpoints:
* http://events.jirafe.com/v1/tracker [POST]
* http://events.jirafe.com/v1/{site-id}/category [POST]
* http://events.jirafe.com/v1/{site-id}/product [POST]
* http://events.jirafe.com/v1/{site-id}/cart [POST]
* http://events.jirafe.com/v1/{site-id}/order [POST]

### The Tracker Endpoint
http://events.jirafe.com/v1/tracker [POST]

The tracker endpoint receives many kinds of events from the beacons.

#### Tracker Wrapper
Each tracker event collection is "wrapped" by an object conforming to the following schema:

##### Schema
```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "http://docs.jirafe.com/schema/event/wrapper-def",
    "type": "object",
    "required" : ["org", "site", "data"],
    "properties": {
        "org": {"type": "string"},
        "site": {"type": "string"},
        "cookie": {"type": "string"},
        "timestamp": {"type": "string", "format": "isodate"},
        "page": {
            "type": "object",
            "properties": {
                "page_type": {"type": "string"},
                "event_type": {"type": "string"},
            },
        },
        "visit": {
            "type": "object", 
            "required": ["id", "visitor", "landing_url", "referrer_url", "user_agent"],
            "properties": {
                "id": {"type": "string"},
                "visitor": {"type": "string"},
                "landing_url": {"type": "string"},
                "referrer_url": {"type": "string"},
                "user_agent": {"type": "string"},
                "attribution": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "required": ["name", "value"],
                        "properties": {
                            "name": {"type": "string"},
                            "value": {"type": "string"}
                        }
                    }
                    "minimum": 1,
                }
            }
        }
    }
    "data": {
        "type": "array",
        "minimum": 1,
        "items": {"oneOf": [
            {"$ref": "http://docs.jirafe.com/schema/event/pageview-def"},
            {"$ref": "http://docs.jirafe.com/schema/event/edit-cart-def"},
            {"$ref": "http://docs.jirafe.com/schema/event/add-to-cart-def"},
            {"$ref": "http://docs.jirafe.com/schema/event/remove-from-cart-def"},
            {"$ref": "http://docs.jirafe.com/schema/event/order-success-def"}
        ]}
    }
}
```

##### Example
```js
{
    "cookie" : // All cookies for domain
    "timestamp":"2013-06-03T16:41:11.600Z",
    "org":1,
    "site":1,
    "data": // Event specific data.
    "page":{
        "current_type":"homepage",
        "referrer_type": "*none*"
        "current_url": "http://localhost:8000/fakewww/index.html",
        "current_title": "Beacon Test Index",
        "referrer_url": "http://localhost:8000/fakewww/products/2/index.html"},
    "visit":{
        "id": "9000628336459776",
        "visitor": "12345678890",
        "landing_url": "http://localhost:8000/fakewww/index.html",
        "referrer_url": "http://www.google.com?s=spectre",
        "user_agent": "Mozilla/5.0 ..."
        "attribution": [{"search": "google"}]
    },
    "customer":{
        "id": "007",
        "lastname": "Bond",
        "firstname": "James"
    },
    "data": ["..."]
}
```

### Tracker Events

#### Pageview Event
When a user views a page, a pageview event should be sent to the Event API.

##### Schema
```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "http://docs.jirafe.com/schema/event/pageview-def",
    "type": "object",
    "required": ["event_type", "page_type"]
    "properties": {
        "page_type": {"enum": ["home", "cart", "order_confirm", "order_success", "checkout", "product", "category", "search"]}
        "event_type": {"enum": ["pageview"]},
        "product": {
            "type": "object",
            "required": ["sku_code", "name", "code", "quantity", "images", "price", "total_price", "currency"],
            "properties": {
                "name": {"type": "string"},
                "sku_code": {"type": "string"},
                "product_id": {"type": "string"},
                "product_code": {"type": "string"},

                "images": {"type": "array", "items": {"type": "string", "format": "uri"}}

                "site_categories": {
                    "type": "array",
                    "items": {
                        "id": {"type": "string"},
                        "name": {"type": "string"}
                    }
                },

                "price": {"type": "string", format: "\^\d\.\d$\\"},
                "currency": {"type": "string", format: "iso4217"}
           }
        },
        "category": {
            "type": "array",
            "items": {
                "id": {"type": "string"},
                "name": {"type": "string"}
            }
        },
    }
}
```

##### Example 1
This is an example of the data which would be transmitted in the case that a user viewed a product page.

```json
{
    "data": [
        {
            "page_type" : "product",
            "event_type" : "pageview",
            "product" : {
                "name": "Tumbler",
                "sku_code": "1",
                "product_id": "1",
                "product_code": "1",
                "images": ["http://example.com/example.png"],
                "description": "Great for shaking up a martini!",
                "categories": [
                    {
                        id: "1",
                        name: "heavy-drinking"
                    }
                ],
                "price": "100",
                "currency": "usd"
            }
        }
    ]
}
```

##### Example 2
This is an example of the data which would be transmitted in the case that a user viewed a category page.

```json
{
    "data": [
        {
            "page_type" : "category",
            "event_type" : "pageview",
            "categories": [
                {
                    id: "1",
                    name: "heavy-drinking"
                }
            ],
            "price": "100",
            "currency": "usd"
        }
    ]
}
```

#### Visit Event
When a user navigates to a store URL from a non-store URL, a visit event should be sent to the Event API.

##### Schema
```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "http://docs.jirafe.com/schema/event/visit-def",
    "type": "object",
    "required": ["event_type"],
    "properties": {
        "event_type": {"enum": ["visit"]}
    }
}
```

#### Add To Cart Event
When a user places an item in their cart, an add_to_cart event should be sent to the Event API.

##### Schema
```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "http://docs.jirafe.com/schema/event/add-to-cart-def",
    "type": "object",
    "properties": {
        "event_type": {"enum": ["visit"]}
        "cart": {
            "type": "object"
            "properties": {
                "id": {"type": "string"}
            }
        },
        "line_items": {
            "type": "array",
            "minimum": 1,
            "items": {
                "type": "object",
                "required": ["sku_code", "name", "code", "quantity", "images", "price", "total_price", "currency"],
                "properties": {
                    "name": {"type": "string"},
                    "sku_code": {"type": "string"},
                    "product_id": {"type": "string"},
                    "product_code": {"type": "string"},

                    "images": {"type": "array", "items": {"type": "string", "format": "uri"}}

                    "site_categories": {
                        "type": "array",
                        "items": {
                            "id": {"type": "string"},
                            "name": {"type": "string"}
                        }
                    },

                    "price": {"type": "string"},
                    "total_price": {"type": "string"},
                    "currency": {"type": "string"}
                }
            }
        }
    }
}
```

#### Remove From Cart Event
When a user removes an item from their cart, a remove_from_cart event should be sent to the Event API.

##### Schema
```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "http://docs.jirafe.com/schema/event/remove-from-cart-def",
    "type": "object",
    "properties": {
        "cart": {
            "type": "object"
            "properties": {
                "id": {"type": "string"},
                "name": {"type": "string"},
            }
        },
        "line_items": {
            "type": "array",
            "minimum": 1,
            "items": {
                "type": "object",
                "required": ["sku_code", "name", "code", "quantity", "images", "price", "total_price", "currency"],
                "properties": {
                    "name": {"type": "string"},
                    "sku_code": {"type": "string"},
                    "product_id": {"type": "string"},
                    "product_code": {"type": "string"},

                    "images": {"type": "array", "items": {"type": "string", "format": "uri"}}

                    "site_categories": {
                        "type": "array",
                        "items": {
                            "id": {"type": "string"},
                            "name": {"type": "string"}
                        }
                    },

                    "price": {"type": "string"},
                    "total_price": {"type": "string"},
                    "currency": {"type": "string"}
                }
            }
        }
    }
}
```

#### Funnel Event
##### Schema
```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "http://docs.jirafe.com/schema/event/funnel-def",
    "type": "object",
    "required": ["event_type", "funnel_name", "step_name", "step_position", "last_step"],
    "properties": {
        "event_type": {"enum": ["funnel"]},
        "funnel_name": {"type": "string"},
        "step_name": {"type": "string"},
        "step_position": {"type": "integer"},
        "last_step": {"type": "boolean"}
    }
}
```

##### Example
```json
{
    "event_type": "funnel",
    "funnel_name": "checkout",
    "step_name": "confirm",
    "step_position": 1,
    "last_step": false
}
```

### The Category Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/category [POST]

#### Schema
```json
{
    "type": "object",
    "required": [ "id", "name", "change_date", "create_date"],
    "properties": {
        "id": {"type": "string"},
        "name": {"type": "string"},
        "change_date": {"type": "string", "format": "date-time"},
        "create_date": {"type": "string", "format": "date-time"}
    }
}
```

#### Additional Constraints
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

#### Example
```json
{
    "id": "3",
    "name": "Tea Accessories",
    "change_date": "2012-01-07T13:24:09",
    "create_date": "2012-01-07T12:24:09"
}
```

#### Code Samples
```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d ‘{“id”:”01134”, “name”:”World”, "change_date":"2012-01-07T13:24:09", "create_date":"2012-01-07T12:24:09"}’ \
       http://events.jirafe.api/v1/{site-id}/{grp-id}/category
```

Should return:
```
200 OK
```

```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d ‘{“id”:”01134”, “name”:”World”, "change_date":"2012-01-07T12:24:09", "create_date":"2012-01-07T13:24:09"}’ \
       http://events.jirafe.api/v1/{site-id}/{grp-id}/category
```

Should return:
```
422 Unprocessable Entity
```
Because the value of the "change_date" field is chronologically before the
value of the "create_date" field.

### The Customer Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/customer [POST]

#### Schema
```json
{
    "type": "object",
    "required": [ "id", "change_date", "create_date"],
    "properties": {
        "id": {"type": "string"},
        "first_name": {"type": "string"},
        "last_name": {"type": "string"},
        "name": {"type": "string"},
        "email": {"type": "string"},
        "phone": {"type": "string"},
        "company": {"type": "string"},
        "department": {"type": "string"},
        "position": {"type": "string"},
        "active": {"type": "boolean"},
        "change_date": {"type": "string", "format": "date-time"},
        "create_date": {"type": "string", "format": "date-time"}
    }
}
```

#### Additional Constraints
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

#### Example
```json
{
    "id": "3",
    "first_name": "John",
    "name": "John Doe",
    "change_date": "2012-01-07T13:24:09",
    "create_date": "2012-01-07T12:24:09"
}
```

#### Code Samples
```
curl -i \
       -H "Content-Type: application/json" \
       -H "Accept: application/json" \
       -X PUT -d '{'id":"3", 'name":"John Doe", "change_date":"2012-01-07T13:24:09", "create_date":"2012-01-07T12:24:09"}' \
       http://events.jirafe.api/v1/{site-id}/{grp-id}/customer
```

Should return:
```
200 OK
```

```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d '{'id":"3", 'name":"John Doe", "change_date":"2012-01-07T12:24:09", "create_date":"2012-01-07T13:24:09"}' \
       http://events.jirafe.api/v1/{site-id}/{grp-id}/customer
```

Should return:
```
422 Unprocessable Entity
```
Because the value of the "change_date" field is chronologically before the
value of the "create_date" field.

### The Product Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/product [POST]

#### Schema
```json
{
    "title": "Product Schema",
    "type" : "object",
    "required": [
        "id",
        "sku",
        "name",
        "brand",
        "cost",
        "price",
        "rating",
        "inventory",
        "create_date",
        "change_date"
    ],
    "properties": {
        "id"         : {"type": "string"},
        "sku"        : {"type": "string"},
        "name"       : {"type": "string"},
        "brand"      : {"type": "string"},
        "cost"       : {"type": "number"},
        "price"      : {"type": "number"},
        "rating"     : {"type": "number"},
        "inventory"  : {"type": "integer"},
        "create_date": {"type": "string", "format": "date-time"},
        "change_date": {"type": "string", "format": "date-time"},
        "urls"       : {
                            "type": "object",
                            "properties": {
                                "admin": {"type": "string", "format": "uri"},
                                "store": {"type": "string", "format": "uri"}
                            }
                        },
        "images"     : {
                            "type": "array",
                            "items": {
                                "type": "object",
                                "required": ["url"],
                                "properties": {
                                    "url": {"type": "string", "format": "uri"}
                                }
                            }
                        },
        "categories" : {
                            "type": "array",
                            "uniqueItems": true,
                            "items": {
                                "type": "object",
                                "required": ["id", "name"],
                                "properties": {
                                    "id"  : {"type": "string"},
                                    "name": {"type": "string"}
                                }
                            }
                        },
        "attributes" : {
                            "type": "array",
                            "uniqueItems": true,
                            "items": {
                                "type": "object",
                                "required": ["id", "name", "value"],
                                "properties": {
                                    "id"  : {"type": "string"},
                                    "name": {"type": "string"},
                                    "value": {"type": "string"}
                                }
                            }
                        }
    }
}
```

#### Notes
* The "cost" field must be a positive value.
* The "price" field must be a positive value.
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

#### Example
```json
{
    "id": "1",
    "sku": "1",
    "name": "Tea Pot",
    "cost": 10.0,
    "price": 100.0,
    "inventory": 3,
    "rating": 9,
    "brand": "ACME",
    "change_date": "2013-04-22T08:57:00",
    "create_date": "2013-04-15T06:33:00",
    "attributes": [
        {
            "id": "1",
            "name": "Size",
            "value": "Short"
        },
        {
            "id": "2",
            "name": "Strength",
            "value": "Stout"
        }
    ],
    "categories": [
        {
            "id": "1",
            "name": "Tea Accessories"
        }
    ],
    "images": [
        {
            "url": "http://example.org/teapot.png"
        }
    ],
    "urls": {
        "store": "http://example.org/store/teapots/1",
        "admin": "http://example.org/admin/teapots/1"
    }
}
```

#### Notes
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

### The Cart Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/cart [POST]

The cart endpoint accepts JSON objects with the following format:
```json
{
    "title": "Cart Schema",
    "type": "object",
    "properties": {
        "id": {"type": "string"},
        "customer_id": {"type": "string"},
        "create_date": {"type": "string", "format": "date-time"},
        "change_date": {"type": "string", "format": "date-time"},

        "total"   : {"type": "number"},
        "subtotal": {"type": "number"},

        "promo"   : {
                        "type": "object",

                        "properties": {
                            "amount": {"type": "number"},
                            "type": {"type": "string"},
                            "code": {"type": "string"},
                        }
                     },
        "items": ...
    }
}
```
#### Notes
* Where the value of the "items" field is an array of objects conforming to the
Product Schema.
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".


### The Order Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/order [POST]

#### Schema
```json
{
    "title": "Order Schema",
    "type": "object",
    "properties": {
        "id": {"type": "string"},
        "cart_id": {"type": "string"},
        "customer": {
            "type": "object",
            "required": ["id", "name"],
            "properties": {
                "id": {"type": "string"},
                "name": {"type": "string"}
            }
        },
        "order_date": {"type": "string", "format": "date-time"},
        "create_date": {"type": "string", "format": "date-time"},
        "change_date": {"type": "string", "format": "date-time"},
        "subtotal": {"type": "number"},
        "total": {"type": "number"},
        "promo": {
            "type": "object",
            "required": ["amount", "type", "code"],
            "properties": {
                "amount": {"type": "number"},
                "type": {"type": "string"},
                "code": {"type": "string"}
            }
        },
        "items": {
            "type": "array",
            "items": {
                "type": "object",
                "required": ["id", "quantity", "promo", "product"],
                "properties": {
                    "id": {"type": "string"},
                    "quantity": {"type": "integer"},
                    "promo": {
                        "type": "object",
                        "required": ["amount", "type", "code"],
                        "properties": {
                            "amount": {"type": "number"},
                            "type": {"type": "string"},
                            "code": {"type": "string"}
                        }
                    }
                },
                "product": ...
            }
        }
}
```

#### Notes
* Where the value of the "product" field is an object conforming to the Product
Schema.
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

