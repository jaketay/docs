---

layout: page
title: Inventory Endpoint
categories: API Documentation
resource: true
description: Sending inventory data to Jirafe API
version: v2
order: 11

---

# Inventory Endpoint

There are two ways to use the inventory endpoint.  You can choose to send inventory events to the Jirafe platform, such as inventory adds or deletes, as they happen.  Additionally, you can choose to send updates to the endpoint nightly to assure daily synchronization and that daily inventory calculations are accurate.

The Jirafe platform will update with the latest 'inventory_level' number received.

### Single Event Example
**URL:** https://event.jirafe.com/v2/{site-id}/inventory [POST]

```
{
    "inventory_level": -1,
    "sku_code":        "SKU8811", 
    "change_date":     "2015-01-23T16:35:38+0000",
    "effective_date":  "2015-01-22"
}
```

### Batch Event Example
**URL:** https://event.jirafe.com/v2/{site-id}/batch [POST]
```
{
    "product.inventory": [
        {
            "inventory_level": -1,
            "sku_code":        "SKU8811", 
            "change_date":     "2015-01-23T16:35:38+0000",
            "effective_date":  "2015-01-20"
        },
        {
            "inventory_level": 11,
            "sku_code":        "SKU8811", 
            "change_date":     "2015-01-23T16:35:38+0000",
            "effective_date":  "2015-01-21"
        },
        {
            "inventory_level": -1,
            "sku_code":        "SKU441", 
            "change_date":     "2015-01-23T16:35:38+0000",
            "effective_date":  "2015-01-22"
        }
    ]
}
```

### JSON Schema
All parameters are optional, unless otherwise indicated.


Property|Description
--- | ---
 sku_code | **required** Sku code
 inventory_level | **required** Inventory quantity for given sku code at end of day
 effective_date | **required** The date for the corresponding end of day inventory_level
 change_date | **required** Timestamp of this event

## Inventory - Things to Consider

* Inventory Data at the Correct Level - Depending on your store's merchandise set-up, you may need to send the inventory data where the product code represents the level in which inventory level is managed.  In other cases, where you have a variety of SKUS for a single product, you should be sending inventory where the product code is set at the SKU level.

* Update Frequency - The rate at which you update the inventory is up to you and the set-up of your system.  Events can be sent in real time as orders are occurring on your system.  Alternatively, you can do a nightly update for each SKU in your system or for reconcilliation.  Additionally, you can choose to do both to ensure accuracy.  The effective_date tells us which date you're providing inventory information for.  The change date is the date you're providing the information or update and is used for auditing and to resolve conflicts in case events are received out of order.

* More than One Order Channels - In the case your business has more than one order channel (eg online, wholesale) and each have respective inventory levels, be sure to update Jirafe with the cumulative number available for a sku code across channels.  Nightly reconcillation can be a good way to do this.

* Fulfillment - In the case your business supports more than one fulfillment method (eg direct, third party drop ship), be sure to update Jirafe with the cumulative number available.  Again, nightly reconciliation can be a good way to do this.

* Low Stock Alert - This lets you override the level which Jirafe should alert you that stock is low. This number can be negative.

* Inventory Alerts - This flag tells us that even though you're tracking inventory for this SKU, you don't want to be notified that inventory is low.

## Inventory - Important Notes
* Jirafe platform will only calculate inventory analysis based on data provided.  For example, if there is 0 inventory for SKU, be sure to update that record with a '0'.  Jirafe WILL NOT assume a 0 with a NULL or blank.

* Jirafe currently does not support more than one inventory calculation for a single SKU code.  For example, where you may have multiple shipments of inventory for a single SKU code.  In this case, just update the endpoint with a single cumulative number for cumulative accuracy.
