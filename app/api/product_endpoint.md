---

layout: page
title: Product Endpoint
categories: API Documentation
resource: true
description: Sending product data to Jirafe API
version: v2
order: 9

---

# Product Endpoint

For every product event, such as add, edit and delete, a product event should be sent to the Jirafe platform.

**URL:** https://event.jirafe.com/v2/{site-id}/product [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/product.json

**Interactive API Tester:** http://docs.jirafe.com/api/event/#!/product

### Example

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

## Products - Things to Consider

Because merchants have various degress of product and merchandise set-ups ranging from standard products, parent products to SKU and configurable products, the Jirafe product object is built flexibly to account for the set-up that makes the most sense for your business.

* Simple Product - In many cases, you may only need a simple product.  For example, if you were selling a stand-alone product, like a CD-Player.  In this case, the product object would be set to is_product=True and is_SKU=True.  There would be no base product.

* Base Product - In the case where you have configurable or varient products, you will need to use a base product that represents of the most parent group product.  For example, if you are selling red, blue, green shirts, the shirt itself is the base product.  In this case, the base product is is_product=True and is_SKU=False.

* Configurable or Varient Product - For each of these product types, you will need to reference the base product per the schema, setting is_product=False and is_SKU=True.  And for each, you will need to inlude the respective attributes (eg red, blue, green) to provide the SKU level attribute detail.

```
NOTE:  In certain cases, your system may support the ability to have multiple levels of variant inheritance. In this case, you will need to provide the correct product and sku mapping.
```