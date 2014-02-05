---
layout: page
title: Using Javascript 
categories: API Documentation
resource: true
version: v1
---

# Javascript Implementation

## Introduction

These instructions outline how to instrument your site with Jirafe's Javascript event tracking if you are implementing Jirafe (or Jirafe's Javascript tracking) via a custom implementation.

If you are installing Jirafe via a standard plug-in and implementation process (Magento, hybris, etc.), these instruction may be helpful but you should refer to the Javascript documentation for that particular installation.

**By following the directions below, you will enable Jirafe to:**

+ Track visits to your store
+ Track pageviews on your store
+ Apply attribution to your visits, orders and revenue
+ Associating the above with individual customers and visitors

**This data is crucial to Jirafe's effectiveness and is used in:**

+ Calculating high-level metrics (# visits, # pageviews, conversion rate, etc.)
+ Generating Marketing reports
+ Generating Site Merchandising reports
+ Enriching the Customer database (with behavior, attribution, etc.)
+ Generating automated Insights

## Jirafe Javascript Library

Place the code in the **head** element of your HTML. Jirafe's Javascript library is loaded asynchronously to avoid impacting your site's page load time.  This code should be on every page that you want Jirafe to track:

```
(function(){
    var d=document,g=d.createElement('script'),s=d.getElementsByTagName('script')[0];
    g.type='text/javascript';g.defer=g.async=true;g.src=d.location.protocol+'//${jirafeApiUrl}';
    s.parentNode.insertBefore(g,s);
})();
var jirafe_site_id = "${jirafeSiteId}";
var jirafe_org_id = "";
```

These values will be provided to you by your Jirafe implementation support contact:

+ The **${jirafeApiUrl}** variable should be set with the Jirafe Javascript API URL.
+ The **${jirafeSiteId}** variable should be set with your Site ID.

The **jirafe_org_id** can be left as an empty string as shown.

## Pageview Tracking
Enable pageview tracking by defining the **jirafe_deferred** function. Place this code in the **head** element of your HTML.

It is important that the logic below is included within the **jirafe_deferred** function, and not outside. This code should be on every page you want to track.

```
function jirafe_deferred(jirafe_api){

    /* Set the page type */
    var type = "...";
    
    /* Set the data object */
    var data = {
        "customer": {
            "id": "10000001"
        }
    };
    
    /*
    Depending on the page type, you will need to set additional metadata.
    
    For example, if type = "product":
    
    data.product = {
        "product_code": "NIKE_AIR_JORDAN_2014",
    	"name": "2014 Nike Air Jordan Sneakers"
    };
    */
    
    /* Send the pageview to Jirafe */
    jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}
```
There are three steps to sending a correct pageview event: 

1. **Set the page type** -- This variable should be set to one of the following values: "product", "category", "search", "order_success", "cart", "other"." You will need to write your own conditional code to identify what type of page the visitor is on and set this variable accordingly.

2. **Set the data object** -- The data object contains additional metadata about the pageview. By default, this object should contain the customer's ID if known. The other required properties depend on the page type of the pageview and are covered in the next section.

3. **Send the pageview to Jirafe** -- You must explicitly call **jirafe_api.pageview(...)** to send Jirafe the pageview event. Since this call exists in the deferred function, it will be sent asynchronously.

## Pageview Tracking -- Examples By Page Type

#### Product

```
function jirafe_deferred(jirafe_api){

	var type = "product";
	data.product = {
    	"product_code": "NIKE_AIR_JORDAN_2014",
    	"name": "2014 Nike Air Jordan Sneakers"
	}

	/* Send the pageview to Jirafe */
	jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}
```
+ **product_code:** the product code of the product. This should be the product code of the parent product (Nike Air Jordan Sneakers), and not the product code of a variant (i.e., size 6 red). Please reach out to your Jirafe support contact if you are unclear on this or have a complicated product hierarchy.
+ **name:** the name of the product

#### Search
```
function jirafe_deferred(jirafe_api){

	var type = "search";
	data.search = {
    	"term": "air jordan shoes",
    	"total_results": "23",
    	"page": "1"
	}

	/* Send the pageview to Jirafe */
	jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}
```
+ **term:** the search term
+ **total_results:** number of products returned by search
+ **total_results:** page number of search results

#### Category
```
function jirafe_deferred(jirafe_api){

    var type = "category";
    data.category = {
        "name": "2014 Nike Air Jordan Sneakers"
    }
    
    /* Send the pageview to Jirafe */
	jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}
```
+ **name:** name of the category

#### Order Success
```
function jirafe_deferred(jirafe_api){

    var type = "order_success";
    /* No additional data properties needed */

    /* However, you do need to send an "order success" event with
    the order number. This is the human-readable order number, 
    NOT the system id of your orders table */
    
    jirafe_api.order.success(jirafe_org_id, jirafe_site_id, {
        "order": {
            "num": "000012"
        }
    });
    
    /* Send the pageview to Jirafe */
	jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}
```
Please note that you must call the order success event and pass in the order number as specified in the example above. 

**This is very important!** Having this event enables Jirafe to attach visit attribution to your orders and revenue.

#### Cart
```
function jirafe_deferred(jirafe_api){
    
    var type = "cart";
    /* No additional data properties needed */
    
    /* Send the pageview to Jirafe */
	jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}
```

#### Other
```
function jirafe_deferred(jirafe_api){

    var type = "other";
    /* No additional data properties needed */
    
    /* Send the pageview to Jirafe */
	jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}       
```
Use this page type for any other pages that don't fit the previous types. In your conditional code, you can simply make this your catch-all.


##Enhanced Visit Attribution Tracking (Recommended)

### Introduction
**By following the instructions above, you will already have standard visit attribution out of the box.**
Jirafe will automatically parse a visit’s landing URL and referring URL to determine where the visit came from based on a list of rules. Any subsequent orders and revenue resulting from the visit will be attributed to the visit’s referring source. 

For example, the default rules will distinguish traffic between:

+ Direct
+ Search engines
+ Social networks
+ External referrers
+ UTM-tagged campaigns

For some merchants, this is sufficient, makes it easy to get started and requires no additional instrumentation.

**However, if you currently employ custom URL parameter tracking for attribution purposes**, you can override Jirafe's default rules with your own. For example, let's say you currently use the following URL parameters, chosen arbitrarily for this example.

+ **ach** (attribution channel)
+ **asch** (attribution subchannel)
+ **asrc** (attribution source). 

In your online ads, marketing emails, content links, social media posts, etc., you could be tagging your inbound links:
**http://www.store.com?ach=email&asch=newsletter&asrc=2014-01-04**

**IMPORTANT NOTE:** If your custom attribution is implemented via Google's UTM tracking (utm_source, utm_medium, utm_campaign, etc.), Jirafe's default implementation already reads these parameters and uses them for reporting. You can skip this entire section. 

### Implementation
The implementation is straightforward. You set the **data.attribution** property with the result of the **jirafe_parseAttribution** function, which is provided as part of the Jirafe JS library that you've already asynchronously loaded.

The **jirafe_parseAttribution** function accepts an array containing your URL parameter names. 

```
function jirafe_deferred(jirafe_api){

    /* Set these variables per early instruction */
	var type = "...";
	data = {...}
	
	data.attribution = jirafe_parseAttribution(["ach","asch","asrc"]);

	/* Send the pageview to Jirafe */
	jirafe_api.pageview(jirafe_org_id, jirafe_site_id, type, data);
}
```

The array can be of any length, however Jirafe's Marketing reports will only drill down to a maximum of 5 levels at this time.

Once the **data.attribution** property is set, Jirafe’s Javascript will then save this property to a browser cookie. Every subsequent event sent via the Javascript event API within the same visit will be automatically decorated with this attribution array.

After the initial pageview, the “attribution” property either should not be set or alternatively set to an empty array [ ].  In both cases we will know not to reset the attribution. More detail on this below.

### Example Visit

**Let's walk through what happens over a series of pageviews by a visitor.**

```
First pageview: <http://www.store.com?ach=email&asch=newsletter&asrc=2014-01-04>

jirafe_parseAttribution(["ach"","asch","asrc"]) will set the attribution property to ["email","newsletter","2014-01-04"]. This property will be sent with the pageview event and from this moment on, the visit will be associated with this attribution. This property will also be saved to the browser cookie and attached to every subsequent event that occurs during the visit.
```

```
Second pageview: <http://www.store.com/categories/dresses>
jirafe_parseAttribution(["ach"","asch","asrc"]) will set the attribution property to an empty array [ ], since no attribution parameters exist in the URL query string. If the Jirafe beacon sees a missing or empty attribution array, it knows not to reset your attribution to an empty array. All subsequent events will still be decorated with ["email","newsletter","2014-01-04"].
```

```
Third pageview: <http://www.store.com/products/red-sundress>
Same as for the second pageview.
```

### Important Notes
1. Do not use attribution parameters that you reuse for other purposes on your store. For example "p" is often used for page number, i.e. "?p=2". You would not want your attribution to be set to "2" every time someone views the second page of your catalog or search results.

2. Visits with landing URLs that do not contain your custom URL parameters will fall back on Jirafe's default attribution rules.

3. If you want to implement more complicated custom attribution tracking, for example some combination of url parameters and custom URL business rules (url parsing, domain matching, etc.), please reach out to your Jirafe Professional Services contact.

4. A pageview that sets new custom attribution values will end the current visit and start a new visit.
