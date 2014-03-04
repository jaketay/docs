---

layout: page
title: Using Javascript 
categories: API Documentation
resource: true
version: v1
order: 9

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
    g.type='text/javascript';g.defer=g.async=true;g.src=d.location.protocol+'//beacon.jirafe.com/jirafe_api.js';
    s.parentNode.insertBefore(g,s);
})();
var jirafe_site_id = "${jirafeSiteId}";
var jirafe_org_id = "";
```

The **${jirafeSiteId}** variable should be set with your **jirafe_site_id**. The **jirafe_org_id** can be left as an empty string as shown.

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

```
http://www.store.com?ach=email&asch=newsletter&asrc=2014-01-04
```

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

## Important Notes & Helpful Tips

1. The product code is the same product code that you are sending in on the your back-end product objects.

2. The order number in your order success javascript must match the order number in your backend order event.

3. You must pass the respective page types for each page. See Javascript reference implementation beliw for page types.

4. The Javascript call generates cookies that you will need to tie to the back end events so that we can tie it together.

5. Do not use attribution parameters that you reuse for other purposes on your store. For example "p" is often used for page number, i.e. "?p=2". You would not want your attribution to be set to "2" every time someone views the second page of your catalog or search results.

6. Visits with landing URLs that do not contain your custom URL parameters will fall back on Jirafe's default attribution rules.

7. If you want to implement more complicated custom attribution tracking, for example some combination of url parameters and custom URL business rules (url parsing, domain matching, etc.), please reach out to your Jirafe Professional Services contact.

8. A pageview that sets new custom attribution values will end the current visit and start a new visit.

## Javascript reference implementation
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
