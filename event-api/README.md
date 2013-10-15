# The Jirafe Events API
In the documentation, some URIs contain the symbol "{site-id}.”
This symbol should be replaced with your *Site Identifier*, which is available from Jirafe support.

Notes:
* All dates are given in ISO 8601:2004 format.
* All dates and times are given in UTC.

## Authentication
When you are granted access to the Jirafe Event-Api, a representative will issue you an oauth token.
In all of the requests described in this document, it is assumed that this token will be sent as a bearer token in the `Authorization` header.

### Example
For example, if you are given an oauth-token of "45dc86d2e2fc4342939a2b6791cfcb9b1c4b89a0", then in all requests you make to the event api you will include this header
```
...
Authorization: Bearer 45dc86d2e2fc4342939a2b6791cfcb9b1c4b89a0
...
```

In some of this document's example code sections, the fake oauth token given above is used.
Please replace this fake token with your own when running the example code.

## Endpoints
The Jirafe Events API exposes the following URI endpoints:

<table>
    <thead>
        <tr>
            <th>URI</th>
            <th>verb</th>
            <th>Schema</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                https://event.jirafe.com/v2/tracker/pageview.gif
            </td>
            <td>
                GET
            </td>
            <td rowspan="2">
                https://event.jirafe.com/v2/schema/pageview.json
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/tracker/pageview
            </td>
            <td>
                POST
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/tracker/visit.gif
            </td>
            <td>
                GET
            </td>
            <td rowspan="2">
                https://event.jirafe.com/v2/schema/visit.json
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/tracker/visit
            </td>
            <td>
                POST
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/{site-id}/cart
            </td>
            <td>
                POST
            </td>
            <td>
                https://event.jirafe.com/v2/schema/cart.json
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/{site-id}/category
            </td>
            <td>
                POST
            </td>
            <td>
                https://event.jirafe.com/v2/schema/category.json
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/{site-id}/employee
            </td>
            <td>
                POST
            </td>
            <td>
                https://event.jirafe.com/v2/schema/employee.json
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/{site-id}/customer
            </td>
            <td>
                POST
            </td>
            <td>
                https://event.jirafe.com/v2/schema/customer.json
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/{site-id}/order
            </td>
            <td>
                POST
            </td>
            <td>
                https://event.jirafe.com/v2/schema/order-placed.json<br/>
                https://event.jirafe.com/v2/schema/order-accepted.json<br/>
                https://event.jirafe.com/v2/schema/order-cancelled.json
            </td>
        </tr>

        <tr>
            <td>
                https://event.jirafe.com/v2/{site-id}/product
            </td>
            <td>
                POST
            </td>
            <td>
                https://event.jirafe.com/v2/schema/product.json
            </td>
        </tr>

    </tbody>
</table>

### The Cart Endpoint
**URL:** https://event.jirafe.com/v2/{site-id}/cart [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/event-api/jsonschema/v2/cart.json

Cart events are sent whenever a user adds, removes or alters their cart. *Note:* this event is distinct from and order event, which is the event that happens when a cart is purchased.

#### Example
```json
{
    "id": "8797436543019",
    "create_date": "2013-06-17T15:16:10.000Z",
    "change_date": "2013-06-17T15:16:15.000Z",
    "subtotal": 99.85,
    "total": 99.85,
    "total_tax": 4.75,
    "total_shipping": 0.0,
    "total_payment_cost": 0.0,
    "total_discounts": 0.0,
    "currency": "USD",
    "cookies": {},
    "items": [
        {
            "id": "8797371007020",
            "create_date": "2013-06-17T15:16:11.000Z",
            "change_date": "2013-06-17T15:16:11.000Z",
            "cart_item_number": "1",
            "quantity": 1,
            "price": 99.85,
            "discount_price": 0.0,
            "product": {
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
        }
    ],
    "previous_items": [
    ],
    "customer": {
        "id": "abc123",
        "create_date": "2013-06-17T15:16:11.000Z",
        "change_date": "2013-06-17T15:16:11.000Z",
        "email": "foo@example.com",
        "first_name": "Jane",
        "last_name": "Doe"
    },
    "visit": {
        "visit_id": "1234",
        "visitor_id": "4321",
        "pageview_id": "5678",
        "last_pageview_id": "8765"
    }
}
```

### The Category Endpoint
**URL:** https://event.jirafe.com/v2/{site-id}/category [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/event-api/jsonschema/v2/category.json

#### Example
```json
{
    "id": "8796094234766",
    "name": "Lens system",
    "change_date": "2013-03-28T19:44:11.000Z",
    "create_date": "2013-03-28T19:38:09.000Z"
}
```

### The Customer Endpoint
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

### The Employee Endpoint
**URL:** https://event.jirafe.com/v2/{site-id}/employee [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/event-api/jsonschema/v2/employee.json

#### Example
```json
{
    "id": "532sdfg",
    "active_flag": true,
    "change_date": "2013-03-28T19:38:03.000Z",
    "create_date": "2013-03-28T19:38:03.000Z",
    "first_name": "Product",
    "last_name": "Manager",
    "email": "product_manager@example.com"
}
```

### The Order Endpoint
**URL:** https://event.jirafe.com/v2/{site-id}/order [POST]

The order endpoint accepts three different schemas:
* The `order-placed` event is sent first with the information connecting the order number with the visit cookies stored by the tracker javascript
* The `order-accepted` event is sent next with all of the information about the order after it has been authorized and paid for by the customer. Anytime the order iterms change, this event is sent again with items containing the new order and previous_items showing how things were before the order changed
* The `order-cancelled` event is sent if the entire order is canceled

**Schema:** https://github.com/jirafe/docs/blob/master/event-api/jsonschema/v2/order-placed.json
#### Example
```json
{
    "order_number": "123456789",
    "cart_id": "123456789",
    "status": "placed",
    "order_date": "2013-06-17T15:16:10.000Z",
    "customer": {
        "id": "abc123",
        "create_date": "2013-06-17T15:16:11.000Z",
        "change_date": "2013-06-17T15:16:11.000Z",
        "email": "foo@example.com",
        "first_name": "Jane",
        "last_name": "Doe"
    },
    "visit": {
        "visit_id": "1234",
        "visitor_id": "4321",
        "pageview_id": "5678",
        "last_pageview_id": "8765"
    }
}
```

**Schema:** https://github.com/jirafe/docs/blob/master/event-api/jsonschema/v2/order-accepted.json
#### Example
```json
{
    "order_number": "8797436543019",
    "cart_id": "1234asdf",
    "status": "accepted",
    "order_date": "2013-06-17T15:16:10.000Z",
    "create_date": "2013-06-17T15:16:10.000Z",
    "change_date": "2013-06-17T15:16:15.000Z",
    "subtotal": 99.85,
    "total": 99.85,
    "total_tax": 4.75,
    "total_shipping": 0.0,
    "total_payment_cost": 0.0,
    "total_discounts": 0.0,
    "currency": "USD",
    "items": [
        {
            "id": "8797371007020",
            "create_date": "2013-06-17T15:16:11.000Z",
            "change_date": "2013-06-17T15:16:11.000Z",
            "status": "accepted",
            "order_item_number": "1",
            "quantity": 1,
            "price": 99.85,
            "discount_price": 0.0,
            "status": "accepted",
            "product": {
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
        }
    ],
    "previous_items": [
    ],
    "customer": {
        "id": "abc123",
        "create_date": "2013-06-17T15:16:11.000Z",
        "change_date": "2013-06-17T15:16:11.000Z",
        "email": "foo@example.com",
        "first_name": "Jane",
        "last_name": "Doe"
    }
}
```

**Schema:** https://github.com/jirafe/docs/blob/master/event-api/jsonschema/v2/order-cancelled.json
#### Example
```json
{
    "order_number": "123456789",
    "cancel_date": "2013-06-17T15:16:10.000Z",
    "status": "cancelled"
}
```

### The Product Endpoint
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

## Error states
On error a json response will be provided with `success: false`, an `error_type`, and an http response code. Below is a list of known response codes and error types:

### 400 bad_json
The json document that you submitted could not be decoded. Your document is most likely malformed.

### 400 invalid_type
The type of document you put is not accepted. The only valid types are `category`, `cart`, `customer`, `product`, `order`.

### 400 validation
The json provided is invalid. Error messages are listed with the corresponding path to the invalid field.

### 403 invalid_token
The `access_token` provided is invalid.

### 403 authorization
The `access_token` provided does not have access to the requested site.

### 503 unknown
An unexpected error occurred. A message will be provided for insight.

## Beacon Javascript reference implementation
Below is a reference example in JSP for the javascript and beacon code. This code block should be placed at the end of your document, just before the closing `</body>` tag.

```jsp
<%@ tag body-content="empty" trimDirectiveWhitespaces="true" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>

<c:if test="${not empty jirafeSiteId}">
<script type="text/javascript">
/* Jirafe */
(function(){
    var d=document,g=d.createElement('script'),s=d.getElementsByTagName('script')[0];
        g.type='text/javascript';g.defer=g.async=true;g.src=d.location.protocol+'//${jirafeApiUrl}';
        s.parentNode.insertBefore(g,s);
})();
var jirafe_site_id = "${jirafeSiteId}";
var jirafe_org_id = "";

function jirafe_deferred(jirafe_api){
    var data = {
        "customer": {
            "id": "${user.uid}",
            "firstname": "${user.firstName}",
            "lastname": "${user.lastName}",
            "email": "${user.uid}"
        }
    };

    /* Custom Marketing Attribution

       You can pass in an array of url parameters to extract from the URL.
       Array position is important, and will be interpreted left-to-right as
       least specific (highest-level attribution) to most specific.

       For example...

       data.attribution = jirafe_parseAttribution(["atr1", "atr2", "atr3", "atr4"]);

       ...will extract url parameters from...

       http://www.store.com?atr1=email&atr2=newsletter&atr3=2013-05&atr4=cta_button

       ...and set data.attribution to the following...

       data.attribution = ["email", "newsletter", "2013-05", "cta_button"];

    */
    data.attribution = jirafe_parseAttribution(
        ["jirafe_atr1", "jirafe_atr2", "jirafe_atr3", "jirafe_atr4", "jirafe_atr5"]);

    <c:choose>
        <c:when test="${pageType == 'PRODUCTSEARCH'}">
            var type = "search";
            data.search = {
                "term": "${searchPageData.freeTextSearch}",
                "total_results": "${searchPageData.pagination.totalNumberOfResults}",
                "page": "${searchPageData.pagination.currentPage + 1}"
            };
        </c:when>

        <c:when test="${pageType == 'PRODUCT'}">
            var type = "product";
            data.product = {
                "product_code":  "${product.code}",
                "name": "${product.name}"
            };
        </c:when>

        <c:when test="${pageType == 'CATEGORY'}">
            var type="category";
            data.category = {
                "name": "${categoryName}"
            };
        </c:when>

        <c:when test="${pageType == 'CART'}">
            var type="cart";
        </c:when>

        <c:when test="${pageType == 'ORDERCONFIRMATION'}">
            jirafe_api.order.success(jirafe_org_id, jirafe_site_id, {
                "order": {
                    "num":  "${orderData.code}"
                }
            });
            var type="order_success";
        </c:when>

        <c:otherwise>
            var type = "other";
        </c:otherwise>
    </c:choose>

    jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}

</script>
</c:if>
```
