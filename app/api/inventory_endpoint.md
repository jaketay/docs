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

**URL:** https://event.jirafe.com/v2/{site-id}/inventory [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/inventory.json

### Example

```
{
    "title": "Product Inventory Schema",
    "type": "object",
    "required": [
        "timestamp",
        "product_code",
        "inventory_level"
    ],
    "properties": {
        "timestamp": {"type": "string", "format": "date-time"},
        "product_code": {"type": "string"},
        "inventory_level": {"type": "integer"}
    }
}
```

## Inventory - Things to Consider

* Send Inventory Data at the Correct Level - Depending on your store's merchandise set-up, you may need to send the inventory data where the product code represents the level in which inventory level is managed.  In other cases, where you have a variety of SKUS for a single product, you should be sending inventory where the product code is set at the SKU level.

* Update Frequency - The rate at which you update the inventory is up to you and the set-up of your system.  Events can be sent in real time as orders are occurring on your system.  Alternatively, you can do a nightly update for each product in your system or for reconcilliation.  Additionally, you can choose to do both to ensure accuracy.

* More than One Order Channels - In the case your business has more than one order channel (eg online, wholesale) and each have respective inventory levels, be sure to update Jirafe with the cumulative number available for a product code across channels.  Nightly reconcillation can be a good way to do this.

* Fulfillment - In the case your business supports more than one fulfillment method (eg direct, third party drop ship), be sure to update Jirafe with the cumulative number available for a product code.  Again, nightly reconciliation can be a good way to do this.

## Inventory - Important Notes
* Jirafe platform will only calculate inventory analysis based on data provided.  For example, if there is 0 inventory for SKU, be sure to update that record with a '0'.  Jirafe WILL NOT assume a 0 with a NULL or blank.

* Jirafe currently does not support more than one inventory calculation for a single product code.  For example, where you may have multiple shipments of inventory for a single product code.  In this case, just update the endpoint with a single cumulative number for cumulative accuracy.


