---

layout: page
title: Cost Endpoint
categories: API Documentation
resource: true
description: Sending cost data to Jirafe API
version: v2
order: 14

---

# Cost Endpoint

This endpoint allows you to send current product cost information to Jirafe.  Cost information is maintained at the SKU level.  The cost is the average product cost for the year.

The Jirafe platform will update with the latest 'sku_cost' received.

**URL:** https://event.jirafe.com/v2/{site-id}/product.cost [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/cost.json

### Example

```
{
    "sku_code":        "SKU8811",
    "sku_cost":        "12.95",
    "change_date":     "2015-01-23T16:35:38+0000",
    "effective_date":  "2015-01-22"
}
```

## Cost - Things to Consider

* Change vs Effective Date - The effective_date tells us which date you're providing cost information for - we currently maintain only a single cost for each sku per year.  The change date is the date you're providing the information or update and is used for auditing and to resolve conflicts in case events are received out of order.

* More than One Order Channel - In the case your business has more than one order channel (eg online, wholesale) and each have respective costs, be sure to update Jirafe with a single average cost for a sku code across channels.

* Fulfillment - In the case your business supports more than one fulfillment method (eg direct, third party drop ship), be sure to update Jirafe with a single average cost for a sku code.

## Cost - Important Notes
* Jirafe currently does not support more than one cost calculation for a single SKU code.  For example, where you may have multiple shipments of inventory for a single SKU code.  In this case, just update the endpoint with a single cumulative number for cumulative accuracy.
