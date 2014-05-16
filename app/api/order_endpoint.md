---

layout: page
title: Order Endpoint
categories: API Documentation
resource: true
description: Sending order data to Jirafe API
version: v2
order: 8

---


# Order Endpoint

The order endpoint accepts three different schemas:

* The `order-placed` event is sent first with the information connecting the order number with the visit cookies stored by the tracker javascript
* The `order-accepted` event is sent next with all of the information about the order after it has been authorized and paid for by the customer. Anytime the order items change, this event is sent again with items containing the new order and previous_items showing how things were before the order changed
* The `order-cancelled` event is sent if the entire order is canceled

**URL:** https://event.jirafe.com/v2/{site-id}/order [POST]


## Order Placed

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/order-placed.json

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

## Order Accepted

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/order-accepted.json

### Example

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
    "delivery_address": {
      "postalcode": "12345",
      "address1": "123 Main St.",
      "country": "US",
      "state": "New York",
      "id": "8797680173079",
      "city": "Anytown"
    },
    "payment_address": {
      "postalcode": "12345",
      "address1": "123 Main St.",
      "country": "US",
      "state": "New York",
      "id": "8797680238615",
      "city": "Anytown"
    },
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

## Order Cancelled

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/order-cancelled.json

### Example

```json
{
    "order_number": "123456789",
    "cancel_date": "2013-06-17T15:16:10.000Z",
    "status": "cancelled"
}
```

## Orders - Things to Consider

The Jirafe order object is flexible to deal with the capabilities of your commerce system.  The different end points can be used to account for a variety of order situations and the best that match your system.  In most cases, there will only be one order with the respective items in that order. This is outlined in the Step 1 below.  

* If your commerce system allows the user to be able to edit or add to an order after it has been placed, you will need to initiate additional order events (as depicted in Steps 2 and 3 below) and send along the previous state of the order.  
* For the ```order accepted``` event, it is important to push both current cart items as well as previous cart items to Jirafe so that Jirafe can maintain an accurate representation of the state of a cart at any point in time during a user visit.  See **Orders Accepted** example below.
* Be sure to add the visit information from the front end cookie to the back end ```order placed``` event so they can be tied together.  See **Visits** example below.
* If you are pushing historical data to Jirafe, be sure to push all of your historical order data to us. The [Batch Endpoint](/api/batch_endpoint 'Batch Endpoint') is ideal for this to do it efficiently.


### Orders Accepted Example:

* Step 1:  Add one item to order. (eg add 1 pair of socks)
	
	
	```
	items:[
		{prod:'socks',qty:1}
	]
	```
* Step 2: Add two more items to order. (eg add 2 pairs of shoes)
	
	```
	items:[
		{prod:'socks',qty:1},
		{prod:'shoes',qty:2}
	],
	previous_items[
		{prod:'socks',qty:1}
	]
	```
* Step 3: Remove items from order. (eg remove 1 pair of socks and 1 pair of shoes)
	
	```
	items:[
		{prod:'shoes',qty:1}
	],
	previous_items[
		{prod:'socks',qty:1},
		{prod:'shoes',qty:2}
	]
	```
	
##Visits Example:
In order to connect front-end user behavior to back-end order data, the Jirafe javascript sets cookies that need to be accessed and passed along with the back end order placed events.  The below schema outlined the json object with each respective value taken from the cookie generated from the javascript.

```
visit: {
    "visit_id": jirafe_vid',
    "visitor_id": 'jirafe_vis',
    "pageview_id": 'jirafe_pvid',
    "last_pageview_id": 'jirafe_lpvid'
}
```