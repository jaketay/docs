---

layout: page
title: API Overview
categories: API Documentation
resource: true
description: How to use our API Events to integrate Jirafe with your app.
image: /img/docs-api.png
type: parent
version: v2
order: 1
index_order: 2
index_title: API Documentation

---

# API Overview

This section provides you all the information you need know in order to use the Jirafe API in order to build your own app!  Whether you are trying use the API to integrate into your own ecommerce site or if your goal is to use our API to power your ecommerce platform with Jirafe, this section will help get you there.  

Jirafe is unlike other analytics offerings you may be know.  It is a combination of both server-side as well as front-end javascript integration.  This allows you to send 100% accurate data as it is represented in your commerce platform and also share information that is not ideal sending via javascript in the browser.

Throughout this documentation, there are a few things to keep in mind:

* Some URIs contain the symbol `{site-id}`.  This symbol should be replaced with your *Site Identifier*, which is part of your authentication credentials and is available from [Support](mailto:support@jirafe.com "Jirafe Support").
* All dates are given in ISO 8601:2004 format.
* All dates and times are given in UTC.

In order for your Jirafe App to work, you must utilize each of the URI end points with required elements (unless otherwise noted) **AND** the Jirafe javascript tage so the Jirafe platform can tie together all of your data and give you a fantastic commerce analytics dashboard.

### Helpful Hints

* When building your application, do not forget about your historical data.  It is a huge advantage to using Jirafe.  Via the API, you can send the Jirafe platform your existing data including:
	* Product catalog,
	* Customer data and their order history,
	* All orders history
* Be sure to tag each of your storefront page types with the appropriate tags.  The [Javascript Implementation](Javascript_Implementation_Guide 'Beacon Javascript Implementation') guide will outline this for you.
* Be sure to use the [Batch API Endpoint](/api/batch_endpoint 'Batch API Endpoint') where it makes sense as it affords huge throughput gains for processing efficiency.


## API Endpoints

The Jirafe Events API exposes the following URI endpoints:

Endpoint | Description | URI
------------ | ------------- | ------------
**GET Pageview** | | `https://event.jirafe.com/v2/tracker/pageview.gif`
**POST Pageview** | | `https://event.jirafe.com/v2/tracker/pageview`
**GET Visit** | | `https://event.jirafe.com/v2/tracker/visit.gif`
**POST Visit** | | `https://event.jirafe.com/v2/tracker/visit`
**[POST Cart](/api/cart_endpoint 'Cart Endpoint')** | Cart events are sent whenever a user adds, removes or alters their cart. *Note:* this event is distinct from and order event, which is the event that happens when a cart is purchased. | `https://event.jirafe.com/v2/{site-id}/cart`
**[POST Category](/api/category_endpoint 'Category Endpoint')** | | `https://event.jirafe.com/v2/{site-id}/category`
**[POST Employee](/api/employee_endpoint 'Employee Endpoint')** | This is not a required end point. | `https://event.jirafe.com/v2/{site-id}/employee`
**[POST Customer](/api/customer_endpoint 'Customer Endpoint')** | | `https://event.jirafe.com/v2/{site-id}/customer`
**[POST Order Placed](/api/order_endpoint 'Order Endpoint')** | The `order-placed` event is sent first with the information connecting the order number with the visit cookies stored by the tracker javascript | `https://event.jirafe.com/v2/{site-id}/order`
**[POST Order Accepted](/api/order_endpoint 'Order Endpoint')** | The `order-accepted` event is sent next with all of the information about the order after it has been authorized and paid for by the customer. Anytime the order iterms change, this event is sent again with items containing the new order and previous_items showing how things were before the order changed |`https://event.jirafe.com/v2/{site-id}/order`
**[POST Order Cancelled](/api/order_endpoint 'Order Endpoint')** | The `order-cancelled` event is sent if the entire order is canceled | `https://event.jirafe.com/v2/{site-id}/order`
**[POST Product](/api/product_endpoint 'Product Endpoint')** | | `https://event.jirafe.com/v2/{site-id}/product`
**[POST Batch](/api/batch_endpoint 'Batch Endpoint')** | This endpoint allows for up to 10MB of events to be submitted in a single request. The object is formed of arrays of the individual endpoints that would normally be sent. | `https://event.jirafe.com/v2/{site-id}/batch`

## Additional Resources

* [Error States](error_states 'Error States')
* [Beacon Javascript Implementation](Javascript_Implementation_Guide 'Beacon Javascript Implementation')
