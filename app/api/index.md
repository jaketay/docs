---
permalink: /v2/
layout: page
title: Jirafe API v2
categories: API Documentation
resource: true
description: How to use our API Events to integrate Jirafe with your app.
image: /img/docs-icon-ph.png
type: parent
---

# Jirafe API v2 Events
In the documentation, some URIs contain the symbol `{site-id}`.
This symbol should be replaced with your *Site Identifier*, which is available from Jirafe support.

*Notes:*

* All dates are given in ISO 8601:2004 format.
* All dates and times are given in UTC.

## Authentication
When you are granted access to the Jirafe Event-Api, a representative will issue you an oauth token.
In all of the requests described in this document, it is assumed that this token will be sent as a bearer token in the `Authorization` header.

### Example
For example, if you are given an oauth-token of **45dc86d2e2fc4342939a2b6791cfcb9b1c4b89a0**, then in all requests you make to the event api you will include this header:

```
...
Authorization: Bearer 45dc86d2e2fc4342939a2b6791cfcb9b1c4b89a0
...
```

In some of this documentation's example code sections, the fake oauth token given above is used.
Please replace this fake token with your own when running the example code.

## Endpoints

The Jirafe Events API exposes the following URI endpoints:

Endpoint | Description | URI
------------ | ------------- | ------------
**GET Pageview** | | `https://event.jirafe.com/v2/tracker/pageview.gif`
**POST Pageview** | | `https://event.jirafe.com/v2/tracker/pageview`
**GET Visit** | | `https://event.jirafe.com/v2/tracker/visit.gif`
**POST Visit** | | `https://event.jirafe.com/v2/tracker/visit`
**[POST Cart](/api/v2/POST/cart_endpoint.html 'Cart Endpoint')** | Cart events are sent whenever a user adds, removes or alters their cart. *Note:* this event is distinct from and order event, which is the event that happens when a cart is purchased. | `https://event.jirafe.com/v2/{site-id}/cart`
**[POST Category](/api/v2/POST/category_endpoint.html 'Category Endpoint')** | | `https://event.jirafe.com/v2/{site-id}/category`
**[POST Employee](/api/v2/POST/employee_endpoint.html 'Employee Endpoint')** | | `https://event.jirafe.com/v2/{site-id}/employee`
**[POST Customer](/api/v2/POST/customer_endpoint.html 'Customer Endpoint')** | | `https://event.jirafe.com/v2/{site-id}/customer`
**[POST Order Placed](/api/v2/POST/order_endpoint.html 'Order Endpoint')** | The `order-placed` event is sent first with the information connecting the order number with the visit cookies stored by the tracker javascript | `https://event.jirafe.com/v2/{site-id}/order`
**[POST Order Accepted](/api/v2/POST/order_endpoint.html 'Order Endpoint')** | The `order-accepted` event is sent next with all of the information about the order after it has been authorized and paid for by the customer. Anytime the order iterms change, this event is sent again with items containing the new order and previous_items showing how things were before the order changed |`https://event.jirafe.com/v2/{site-id}/order`
**[POST Order Cancelled](/api/v2/POST/order_endpoint.html 'Order Endpoint')** | The `order-cancelled` event is sent if the entire order is canceled | `https://event.jirafe.com/v2/{site-id}/order`
**[POST Product](/api/v2/POST/product_endpoint.html 'Product Endpoint')** | | `https://event.jirafe.com/v2/{site-id}/product`
**[POST Batch](/api/v2/POST/batch_endpoint.html 'Batch Endpoint')** | This endpoint allows for up to 10MB of events to be submitted in a single request. The object is formed of arrays of the individual endpoints that would normally be sent. | `https://event.jirafe.com/v2/{site-id}/batch`

## Additional Resources

* [Error States](error_states.index.html 'Error States')
* [Beacon Javascript Implementation](beacon_javascript.html 'Beacon Javascript Implementation')
