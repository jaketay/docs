---

layout: page
title: Cart Endpoint
categories: API Documentation
resource: true
description: Sending cart data to Jirafe API
version: v2
order: 4

---

# Cart Endpoint

Cart events are sent whenever a user adds, removes or alters their cart. *Note:* this event is distinct from and order event, which is the event that happens when a cart is purchased.

**URL:** https://event.jirafe.com/v2/{site-id}/cart [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/cart.json


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

## Carts - Things to Consider

* For cart events, it is important to push both current cart items as well as previous cart items to Jirafe so that Jirafe can maintain an accurate representation of the state of a cart at any point in time during a user visit.  See **Cart Items** example below.
* Be sure to add the visit information from the front end cookie to the back end cart event so they can be tied together.  See **Visits** example below.
* If you are pushing historical data to Jirafe, we recommend not pushing cart data as carts are only analyzed on a going forward basis.


### Cart Items Example:

* Step 1:  Add one item to cart. (eg add 1 pair of socks)
	
	```
	items:[
		{prod:'socks',qty:1}
	]
	```
* Step 2: Add two more items to cart. (eg add 2 pairs of shoes)
	
	```
	items:[
		{prod:'socks',qty:1},
		{prod:'shoes',qty:2}
	],
	previous_items[
		{prod:'socks',qty:1}
	]
	```
* Step 3: Remove items from cart. (eg remove 1 pair of socks and 1 pair of shoes)
	
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
In order to connect front-end user behavior to back-end cart data, the Jirafe javascript sets cookies that need to be accessed and passed along with the back end cart events.  The below schema outlined the json object with each respective value taken from the cookie generated from the javascript.

```
visit: {
    "visit_id": jirafe_vid',
    "visitor_id": 'jirafe_vis',
    "pageview_id": 'jirafe_pvid',
    "last_pageview_id": 'jirafe_lpvid'
}
```