# The Jirafe Events API

In the documentation, some URIs contain the symbols “{org-id}” and “{site-id}.”
These symbols should be replaced with your *Organization Identifier* and your *Site Identifier*, which are available from Jirafe support.

Notes:
* Currently all fields are required fields.
* All dates are given in ISO 8601:2004 format.
* All dates and times are given in UTC.

## Endpoints
The Jirafe Events API exposes the following URI endpoints:
* http://events.jirafe.com/v1/{org-id}/{site-id}/category [POST]
* http://events.jirafe.com/v1/{org-id}/{site-id}/product [POST]
* http://events.jirafe.com/v1/{org-id}/{site-id}/cart [POST]
* http://events.jirafe.com/v1/{org-id}/{site-id}/order [POST]

### The Category Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/category [POST]

#### Schema
```json
{
    "type" : "object",
    "required" : [ "id", "name", "change_date", "create_date"],
    "properties" : {
        "id" : {"type" : "string"},
        "name" : {"type" : "string"},
        "change_date" : {"type" : "string", "format" : "date-time"},
        "create_date" : {"type" : "string", "format" : "date-time"}
    }
}
```

#### Additional Constraints
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

#### Example
```json
{
    "id" : "3",
    "name" : "Tea Accessories",
    "change_date" : "2012-01-07T13:24:09",
    "create_date" : "2012-01-07T12:24:09"
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

### The Product Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/product [POST]

#### Schema
```json
{
    "title" : "Product Schema",
    "type"  : "object",
    "required" : [
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
    "properties" : {
        "id"          : {"type" : "string"},
        "sku"         : {"type" : "string"},
        "name"        : {"type" : "string"},
        "brand"       : {"type" : "string"},
        "cost"        : {"type" : "number"},
        "price"       : {"type" : "number"},
        "rating"      : {"type" : "number"},
        "inventory"   : {"type" : "integer"},
        "create_date" : {"type" : "string", "format" : "date-time"},
        "change_date" : {"type" : "string", "format" : "date-time"},
        "urls"        : {
                            "type" : "object",
                            "properties" : {
                                "admin" : {"type" : "string", "format" : "uri"},
                                "store" : {"type" : "string", "format" : "uri"}
                            }
                        },
        "images"      : {
                            "type" : "array",
                            "items" : {
                                "type" : "object",
                                "required" : ["url"],
                                "properties" : {
                                    "url" : {"type" : "string", "format" : "uri"}
                                }
                            }
                        },
        "categories"  : {
                            "type" : "array",
                            "uniqueItems": True,
                            "items" : {
                                "type" : "object",
                                "required" : ["id", "name"],
                                "properties" : {
                                    "id"   : {"type" : "string"},
                                    "name" : {"type" : "string"}
                                }
                            }
                        },
        "attributes"  : {
                            "type" : "array",
                            "uniqueItems": True,
                            "items" : {
                                "type" : "object",
                                "required" : ["id", "name", "value"],
                                "properties" : {
                                    "id"   : {"type" : "string"},
                                    "name" : {"type" : "string"},
                                    "value" : {"type" : "string"}
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

### The Cart Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/cart [POST]

The cart endpoint accepts JSON objects with the following format:
```json
{
    "title" : "Cart Schema",
    "type" : "object",
    "properties" : {
        "id" : {"type" : "string"},
        "customer_id" : {"type" : "string"},
        "create_date" : {"type" : "string", "format" : "date-time"},
        "change_date" : {"type" : "string", "format" : "date-time"},

        "total"    : {"type" : "number"},
        "subtotal" : {"type" : "number"},

        "promo"    : {
                        "type" : "object",

                        "properties" : {
                            "amount" : {"type" : "number"},
                            "type" : {"type" : "string"},
                            "code" : {"type" : "string"},
                        }
                     },
        "items" : ...
    }
}
```
#### Notes
Where the value of the "items" field is an array of objects conforming to the
Product Schema.


### The Order Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/order [POST]

#### Schema
```json
{
    "title" : "Order Schema",
    "type" : "object",
    "properties" : {
        "id" : {"type" : "string"},
        "cart_id" : {"type" : "string"},
        "customer" : {
            "type" : "object",
            "required" : ["id", "name"],
            "properties" : {
                "id" : {"type" : "string"},
                "name" : {"type" : "string"}
            }
        },
        "order_date" : {"type" : "string", "format" : "date-time"},
        "create_date" : {"type" : "string", "format" : "date-time"},
        "change_date" : {"type" : "string", "format" : "date-time"},
        "subtotal" : {"type" : "number"},
        "total" : {"type" : "number"},
        "promo" : {
            "type" : "object",
            "required" : ["amount", "type", "code"],
            "properties" : {
                "amount" : {"type" : "number"},
                "type" : {"type" : "string"},
                "code" : {"type" : "string"}
            }
        },
        "items" : {
            "type" : "array",
            "items" : {
                "type" : "object",
                "required" : ["id", "quantity", "promo", "product"],
                "properties" : {
                    "id" : {"type" : "string"},
                    "quantity" : {"type" : "integer"},
                    "promo" : {
                        "type" : "object",
                        "required" : ["amount", "type", "code"],
                        "properties" : {
                            "amount" : {"type" : "number"},
                            "type" : {"type" : "string"},
                            "code" : {"type" : "string"}
                        }
                    }
                },
                "product" : ...
            }
        }
}
```

#### Notes
Where the value of the "product" field is an object conforming to the Product
Schema.
