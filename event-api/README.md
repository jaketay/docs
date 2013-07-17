# The Jirafe Events API

In the documentation, some URIs contain the symbols “{org-id}” and “{site-id}.”
These symbols should be replaced with your *Organization Identifier* and your *Site Identifier*, which are available from Jirafe support.

Notes:
* All dates are given in ISO 8601:2004 format.
* All dates and times are given in UTC.

## Endpoints
The Jirafe Events API exposes the following URI endpoints:
* http://events.jirafe.com/v1/tracker [POST]
* http://events.jirafe.com/v1/{site-id}/cart [POST]
* http://events.jirafe.com/v1/{site-id}/category [POST]
* http://events.jirafe.com/v1/{site-id}/customer [POST]
* http://events.jirafe.com/v1/{site-id}/employee [POST]
* http://events.jirafe.com/v1/{site-id}/order [POST]
* http://events.jirafe.com/v1/{site-id}/product [POST]

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
                "tag_attribution": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    },
                    "minimum": 1,
                    "maximum": 5
                },
                "rule_attribution": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    },
                    "minimum": 1,
                    "maximum": 5
                }
            }
        }
    },
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
```json
{
    "cookie" : "All cookies for domain",
    "timestamp":"2013-06-03T16:41:11.600Z",
    "org":1,
    "site":1,
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
        "attribution": ["search", "google", "cats"]
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
    "required": ["event_type", "page_type"],
    "properties": {
        "page_type": {"enum": ["homepage", "cart", "order_confirm", "order_success", "checkout", "product", "category", "search", "other"]},
        "event_type": {"enum": ["pageview"]},
        "product": {
            "type": "object",
            "required": ["product_code"],
            "properties": {
                "name": {"type": "string"},
                "sku_code": {"type": "string"},
                "product_code": {"type": "string"}
            }
        },
        "customer": {
            "type": "array",    
            "items": {
                "id": {"type": "string"},
                "lastname": {"type": "string"},
                "firstname": {"type": "string"},
                "email": {"type": "string", "format": "email"}
            }
        },
        "category": {
            "type": "object",
            "items": {
                "id": {"type": "string"},
                "name": {"type": "string"}
            }
        },
        "search": {
            "type": "object",
            "properties": {
                "keyword": {"type": "string"},
                "page": {"type": "integer"},
                "total_results": {"type": "integer"}
            }
        }, 
        "attribution": {
            "type": "array",
            "minimum": 1,
            "items" : {"type": "string"},
        }
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
                "product_code": "1"
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
            "category": {
                    "id": "1",
                    "name": "adorable puppies"
            }
        }
    ]
}
```

##### Example 3
This is an example of the data which would be transmitted in the case that a user viewed a search page.

```json
{
    "data": [
        {
            "page_type" : "search",
            "event_type" : "pageview",
            "search": {
                "keyword": "blue suede shoes",
                "page": "1",
                "total_results": "100"
            }
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
    "required": ["event_type", "line_item"],
    "properties": {
        "event_type": {"enum": ["add_item_to_cart"]},
        "cart": {
            "type": "object",
            "properties": {
                "id": {"type": "string"}
            }
        },
        "line_item": {
            "type": "object",
            "required": ["sku_code", "quantity", "total_price", "currency"],
            "properties": {
                "name": {"type": "string"},
                "sku_code": {"type": "string"},
                "product_code": {"type": "string"},
                "images": {"type": "array", "items": {"type": "string", "format": "uri"}},
                "price": {"type": "string"},
                "quantity": {"type": "string"},
                "total_price": {"type": "string"},
                "currency": {"type": "string"}
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
    "required": ["event_type", "line_item"],
    "properties": {
        "event_type": {"enum": ["remove_item_from_cart"]},
        "cart": {
            "type": "object",
            "properties": {
                "id": {"type": "string"}
            }
        },
        "line_item": {
            "type": "object",
            "required": ["sku_code", "quantity", "total_price", "currency"],
            "properties": {
                "name": {"type": "string"},
                "sku_code": {"type": "string"},
                "product_code": {"type": "string"},
                "images": {"type": "array", "items": {"type": "string", "format": "uri"}},
                "price": {"type": "string"},
                "quantity": {"type": "string"},
                "total_price": {"type": "string"},
                "currency": {"type": "string"}
            }
        }
    }
}
```

#### Edit Cart Event
When a user removes an item from their cart, an edit cart event should be sent to the Event API.

##### Schema
```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "http://docs.jirafe.com/schema/event/edit-cart-def",
    "type": "object",
    "required": ["event_type", "line_item"],
    "properties": {
        "event_type": {"enum": ["edit_cart"]},
        "cart": {
            "type": "object",
            "properties": {
                "id": {"type": "string"}
            }
        },
        "line_item": {
            "type": "object",
            "required": ["sku_code", "quantity", "total_price", "currency"],
            "properties": {
                "name": {"type": "string"},
                "sku_code": {"type": "string"},
                "product_code": {"type": "string"},
                "images": {"type": "array", "items": {"type": "string", "format": "uri"}},
                "price": {"type": "string"},
                "quantity": {"type": "string"},
                "total_price": {"type": "string"},
                "currency": {"type": "string"}
            }
        }
    }
}
```

#### Order Success Event
When an order is completed and the browser loads the post-order confirmation/success/receipt page, an order_success event should be sent to the Event API.

##### Schema
```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "http://docs.jirafe.com/schema/event/order-success-def",
    "type": "object",
    "required": ["event_type", "order"],
    "properties": {
        "event_type": {"enum": ["order_success"]},
        "order": {
            "type": "object",
            "required": ["num"],
            "properties": {
                "id": {"type": "string"},
                "num": {"type": "string"}
            }
        }
    }
}
```

##### Example
```json
{
    "data": [
        {
            "id": "12345",
            "num": "0000000010"
        }
    ]
}
```


#### Funnel Event
When a funnel event occurs, a funnel event should be sent to the Event API. 

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
    "data": [
        {
            "event_type": "funnel",
            "funnel_name": "checkout",
            "step_name": "confirm",
            "step_position": 1,
            "last_step": false
        }
    ]
}
```

### The Cart Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/cart [POST]

The cart endpoint accepts JSON objects with the following format:
```json
{
    "title" : "Cart Schema",
    "type" : "object",
    "required" : [
        "id",
        "order_number",
        "create_date",
        "change_date",
        "subtotal",
        "total",
        "total_tax",
        "total_shipping",
        "total_payment_cost",
        "total_discounts",
        "currency",
    ],
    "properties" : {
        "id" : {"type" : "string"},
        "order_number" : {"type" : "string"},
        "cart_id" : {"type" : "string"},
        "customer_id" : {"type" : "string"},
        "create_date" : {"type" : "string", "format" : "date-time"},
        "change_date" : {"type" : "string", "format" : "date-time"},
        "status" : {"type" : "string"},
        "subtotal" : {"type" : "number"},
        "total" : {"type" : "number"},
        "total_tax" : {"type" : "number"},
        "total_shipping" : {"type" : "number"},
        "total_payment_cost" : {"type" : "number"},
        "total_discounts" : {"type" : "number"},
        "currency" : {"type" : "string"},
        "items" : {"type" : "array", "items" : {
            "type" : "object",
            "required" : [
                "id",
                "create_date",
                "change_date",
                "order_item_number",
                "quantity",
                "amount",
                "discount_amount",
                "product"
            ],
            "properties" : {
                "id" : {"type" : "string"},
                "create_date" : {"type" : "string", "format" : "date-time"},
                "change_date" : {"type" : "string", "format" : "date-time"},
                "order_item_number" : {"type" : "string"},
                "status" : {"type" : "string"},
                "quantity" : {"type" : "integer"},
                "amount" : {"type" : "number"},
                "discount_amount" : {"type" : "number"},
                "product" : ["..."]
            }
        }

    }
}
```

#### Notes
* The value of the "product" field within the "items" array is
an object conforming to the Product Schema.
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

#### Example
```json
{
    "change_date": "2013-06-17T15:16:15.000Z", 
    "create_date": "2013-06-17T15:16:10.000Z", 
    "currency": "USD", 
    "customer_id": "john.doe@gmail.com", 
    "id": "8797436543019", 
    "items": [
        {
            "amount": 99.85, 
            "change_date": "2013-06-17T15:16:11.000Z", 
            "create_date": "2013-06-17T15:16:11.000Z", 
            "discount_amount": 0.0, 
            "id": "8797371007020", 
            "order_item_number": "0", 
            "product": {
                "brand": "Canon", 
                "catalog": {
                    "id": "electronicsProductCatalog", 
                    "name": "Electronics Product Catalog", 
                    "version_id": "Online"
                }, 
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
                "change_date": "2013-03-28T19:50:58.000Z", 
                "code": "1934793", 
                "create_date": "2013-03-28T19:46:39.000Z", 
                "id": "8796107014145", 
                "images": [
                    {
                        "url": "http://hybris.jirafe.com/medias/sys_master/images/8796224651294/1934793_1719.jpg"
                    }
                ], 
                "is_product": true, 
                "is_sku": true, 
                "name": "PowerShot A480", 
                "rating": 5.0, 
                "vendors": [
                    {
                        "id": "8796093089750", 
                        "name": "Electro"
                    }
                ]
            }, 
            "quantity": 1, 
            "status": "other"
        }
    ], 
    "order_number": "00041000", 
    "status": "other", 
    "subtotal": 99.85, 
    "total": 99.85, 
    "total_discounts": 0.0, 
    "total_payment_cost": 0.0, 
    "total_shipping": 0.0, 
    "total_tax": 4.75
}
```

#### Code Samples
```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d ‘...’ \
       http://events.jirafe.com/v1/{site-id}/{grp-id}/cart
```

Should return:
```
200 OK
```


### The Category Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/category [POST]

#### Schema
```json
{
    "title" : "Category Schema",
    "type" : "object",
    "required" : [ "id", "name", "change_date", "create_date"],
    "properties" : {
        "id" : {"type" : "string"},
        "name" : {"type" : "string"},
        "change_date" : {"type" : "string", "format" : "date-time"},
        "create_date" : {"type" : "string", "format" : "date-time"},
        "parent_categories" : {"type" : "array", "items" : {
            "type" : "object",
            "required" : ["id"],
            "properties" : {
                "id" : {"type" : "string"},
            }
        }},
    }
}
```

#### Additional Constraints
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

#### Example
```json
{
    "change_date": "2013-03-28T19:44:11.000Z", 
    "create_date": "2013-03-28T19:38:09.000Z", 
    "id": "8796094234766", 
    "name": "Lens system"
}
```

#### Code Samples
```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d ‘{
    "change_date": "2013-03-28T19:44:11.000Z", 
    "create_date": "2013-03-28T19:38:09.000Z", 
    "id": "8796094234766", 
    "name": "Lens system"
}’ \
       http://events.jirafe.com/v1/{site-id}/{grp-id}/category
```

Should return:
```
200 OK
```

```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d ‘{
    "change_date": "2012-03-28T19:44:11.000Z", 
    "create_date": "2013-03-28T19:38:09.000Z", 
    "id": "8796094234766", 
    "name": "Lens system"
}’ \
       http://events.jirafe.com/v1/{site-id}/{grp-id}/category
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
    "title" : "Customer Schema",
    "type" : "object",
    "required" : [
        "create_date",
        "change_date",
        "id",
    ],
    "properties" : {
        "create_date" : {"type" : "string", "format" : "date-time"},
        "change_date" : {"type" : "string", "format" : "date-time"},
        "id" : {"type" : "string"},
        "email" : {"type" : "string"},
        "name" : {"type" : "string"},
        "phone" : {"type" : "string"},
        "first_name" : {"type" : "string"},
        "last_name" : {"type" : "string"},
        "active_flag" : {"type" : "boolean", "default" : true},
        "company" : {"type" : "string"},
        "department" : {"type" : "string"},
        "position" : {"type" : "string"},
        "marketing_opt_in" : {"type" : "boolean", "default" : false},
    }
}
```

#### Additional Constraints
* The value of the field "create_date" must not be chronologically
later than the value of the field "change_date".

#### Example
```json
{
    "active_flag": true, 
    "change_date": "2013-06-17T15:15:53.000Z", 
    "create_date": "2013-06-17T15:15:53.000Z", 
    "email": "john.doe@gmail.com", 
    "first_name": "John", 
    "id": "john.doe@gmail.com", 
    "last_name": "Doe", 
    "name": "John Doe"
}
```

#### Code Samples
```
curl -i \
       -H "Content-Type: application/json" \
       -H "Accept: application/json" \
       -X PUT -d '{
    "active_flag": true, 
    "change_date": "2013-06-17T15:15:53.000Z", 
    "create_date": "2013-06-17T15:15:53.000Z", 
    "email": "john.doe@gmail.com", 
    "first_name": "John", 
    "id": "john.doe@gmail.com", 
    "last_name": "Doe", 
    "name": "John Doe"
}' \
       http://events.jirafe.com/v1/{site-id}/{grp-id}/customer
```

Should return:
```
200 OK
```

```
curl -i \
       -H "Content-Type: application/json" \
       -H "Accept: application/json" \
       -X PUT -d '{
    "active_flag": true, 
    "change_date": "2012-06-17T15:15:53.000Z", 
    "create_date": "2013-06-17T15:15:53.000Z", 
    "email": "john.doe@gmail.com", 
    "first_name": "John", 
    "id": "john.doe@gmail.com", 
    "last_name": "Doe", 
    "name": "John Doe"
}' \
       http://events.jirafe.com/v1/{site-id}/{grp-id}/customer
```

Should return:
```
422 Unprocessable Entity
```
Because the value of the "change_date" field is chronologically before the
value of the "create_date" field.


### The Employee Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/employee [POST]

#### Schema
```json
{
    "title" : "Employee Schema",
    "type" : "object",
    "required" : [
        "create_date",
        "change_date",
        "id",
        "email"
    ],
    "properties" : {
        "create_date" : {"type" : "string", "format" : "date-time"},
        "change_date" : {"type" : "string", "format" : "date-time"},
        "id" : {"type" : "string"},
        "email" : {"type" : "string"},
        "name" : {"type" : "string"},
        "phone" : {"type" : "string"},
        "first_name" : {"type" : "string"},
        "last_name" : {"type" : "string"},
        "active_flag" : {"type" : "boolean", "default" : true},
        "company" : {"type" : "string"},
        "department" : {"type" : "string"},
        "position" : {"type" : "string"},
        "marketing_opt_in" : {"type" : "boolean", "default" : false}
    }
}
```

#### Additional Constraints
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

#### Example
```json
{
    "active_flag": true, 
    "change_date": "2013-03-28T19:38:03.000Z", 
    "create_date": "2013-03-28T19:38:03.000Z", 
    "first_name": "Product", 
    "id": "productmanager", 
    "last_name": "Manager", 
    "name": "Product Manager"
}
```

#### Code Samples
```
curl -i \
       -H "Content-Type: application/json" \
       -H "Accept: application/json" \
       -X PUT -d '{
    "active_flag": true, 
    "change_date": "2013-03-28T19:38:03.000Z", 
    "create_date": "2013-03-28T19:38:03.000Z", 
    "first_name": "Product", 
    "id": "productmanager", 
    "last_name": "Manager", 
    "name": "Product Manager"
}' http://events.jirafe.com/v1/{site-id}/{grp-id}/employee
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
       http://events.jirafe.com/v1/{site-id}/{grp-id}/customer
```

Should return:
```
422 Unprocessable Entity
```
Because the value of the "change_date" field is chronologically before the
value of the "create_date" field.


### The Order Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/order [POST]

#### Schema
```json
{
    "title" : "Order Schema",
    "type" : "object",
    "required" : [
        "id",
        "order_number",
        "customer_id",
        "order_date",
        "create_date",
        "change_date",
        "status",
        "subtotal",
        "total",
        "total_tax",
        "total_shipping",
        "total_payment_cost",
        "total_discounts",
        "currency",
    ],
    "properties" : {
        "id" : {"type" : "string"},
        "order_number" : {"type" : "string"},
        "cart_id" : {"type" : "string"},
        "customer_id" : {"type" : "string"},
        "order_date" : {"type" : "string", "format" : "date-time"},
        "create_date" : {"type" : "string", "format" : "date-time"},
        "change_date" : {"type" : "string", "format" : "date-time"},
        "status" : {"type" : "string"},
        "subtotal" : {"type" : "number"},
        "total" : {"type" : "number"},
        "total_tax" : {"type" : "number"},
        "total_shipping" : {"type" : "number"},
        "total_payment_cost" : {"type" : "number"},
        "total_discounts" : {"type" : "number"},
        "currency" : {"type" : "string"},
        "items" : {"type" : "array", "items" : {
            "type" : "object",
            "required" : [
                "id",
                "create_date",
                "change_date",
                "order_item_number",
                "status",
                "quantity",
                "amount",
                "discount_amount",
                "product"
            ],
            "properties" : {
                "id" : {"type" : "string"},
                "create_date" : {"type" : "string", "format" : "date-time"},
                "change_date" : {"type" : "string", "format" : "date-time"},
                "order_item_number" : {"type" : "string"},
                "status" : {"type" : "string"},
                "quantity" : {"type" : "integer"},
                "amount" : {"type" : "number"},
                "discount_amount" : {"type" : "number"},
                "product" : ["..."]
            }
        }
    }
}
```

#### Notes
* Where the value of the "product" field is an object conforming to the Product
Schema.
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".

#### Example
```json
{
    "change_date": "2013-06-17T15:34:13.000Z", 
    "create_date": "2013-06-17T15:34:03.000Z", 
    "currency": "USD", 
    "customer_id": "john.doe@gmail.com", 
    "id": "8796420735021", 
    "items": [
        {
            "amount": 99.85, 
            "change_date": "2013-06-17T15:34:04.000Z", 
            "create_date": "2013-06-17T15:33:13.000Z", 
            "discount_amount": 0.0, 
            "id": "8796420735022", 
            "order_item_number": "0", 
            "product": {
                "brand": "Canon", 
                "catalog": {
                    "id": "electronicsProductCatalog", 
                    "name": "Electronics Product Catalog", 
                    "version_id": "Online"
                }, 
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
                "change_date": "2013-03-28T19:50:58.000Z", 
                "code": "1934793", 
                "create_date": "2013-03-28T19:46:39.000Z", 
                "id": "8796107014145", 
                "images": [
                    {
                        "url": "http://hybris.jirafe.com/medias/sys_master/images/8796224651294/1934793_1719.jpg"
                    }
                ], 
                "is_product": true, 
                "is_sku": true, 
                "name": "PowerShot A480", 
                "rating": 5.0, 
                "vendors": [
                    {
                        "id": "8796093089750", 
                        "name": "Electro"
                    }
                ]
            }, 
            "quantity": 1, 
            "status": "pending"
        }
    ], 
    "order_date": "2013-06-17T15:34:03.000Z", 
    "order_number": "00041002", 
    "status": "pending", 
    "subtotal": 99.85, 
    "total": 111.84, 
    "total_discounts": 0.0, 
    "total_payment_cost": 0.0, 
    "total_shipping": 11.99, 
    "total_tax": 5.33
}
```

#### Code Samples
```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d ‘...’ \
       http://events.jirafe.com/v1/{site-id}/{grp-id}/order
```

Should return:
```
200 OK
```


### The Product Endpoint
http://events.jirafe.com/v1/{org-id}/{site-id}/product [POST]

#### Schema
```json
{
    "title" : "Product Schema",
    "type"  : "object",
    "required" : [
        "id",
        "is_product",
        "is_sku",
        "code",
        "create_date",
        "change_date"
    ],
    "properties" : {
        "id"          : {"type" : "string"},
        "is_product"  : {"type" : "boolean"},
        "is_sku"      : {"type" : "boolean"},

        "catalog"     : {
            "type" : "object",
            "required" : ["id"],
            "properties" : {
                "id" : {"type" : "string"},
                "version_id" : {"type" : "string"},
                "name" : {"type" : "string"},
            }
        },

        "name"        : {"type" : "string"},
        "code"        : {"type" : "string"},
        "ancestors"   : {"type" : "array", "items" : {"type": "string"}},

        "base_product": {
            "type" : "object",
            "properties" : {
                "id" : {"type" : "string"},
                "code" : {"type" : "string"},
                "name" : {"type" : "string"},
            }
        },
        "vendors"     : {
            "type" : "array",
            "items" : {
                "type" : "object",
                "properties" : {
                    "id" : {"type" : "string"},
                    "name" : {"type" : "string"},
                }
            }
        },

        "brand"       : {"type" : "string"},
        "rating"      : {"type" : "number"},

        "create_date" : {"type" : "string", "format" : "date-time"},
        "change_date" : {"type" : "string", "format" : "date-time"},

        "urls"        : {
            "type" : "object",
            "properties" : {
                "admin" : {"type" : "string", "format" : "uri"},
                "store" : {"type" : "string", "format" : "uri"},
            }
        },
        "images"      : {
            "type" : "array",
            "items" : {
                "type" : "object",
                "required" : ["url"],
                "properties" : {
                    "url" : {"type" : "string", "format" : "uri"},
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
                    "name" : {"type" : "string"},
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
                    "id"    : {"type" : "string"},
                    "name"  : {"type" : "string"},
                    "value" : {"type" : "string"},
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
    "brand": "Canon", 
    "catalog": {
        "id": "electronicsProductCatalog", 
        "name": "Electronics Product Catalog", 
        "version_id": "Online"
    }, 
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
    "change_date": "2013-03-28T19:50:58.000Z", 
    "code": "1934793", 
    "create_date": "2013-03-28T19:46:39.000Z", 
    "id": "8796107014145", 
    "images": [
        {
            "url": "http://hybris.jirafe.com/medias/sys_master/images/8796224651294/1934793_1719.jpg"
        }
    ], 
    "is_product": true, 
    "is_sku": true, 
    "name": "PowerShot A480", 
    "rating": 5.0, 
    "vendors": [
        {
            "id": "8796093089750", 
            "name": "Electro"
        }
    ]
}
```

#### Notes
* The value of the field "create_date" must be chronologically before the value
of the field "change_date".


#### Code Samples
```
curl -i \
       -H "Content-Type: application/json” \
       -H “Accept: application/json" \
       -X PUT -d ‘...’ \
       http://events.jirafe.com/v1/{site-id}/{grp-id}/product
```

Should return:
```
200 OK
```
