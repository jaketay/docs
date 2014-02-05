---
layout: page
title: Javascript - Optional Tracking
categories: Plugin Installation
resource: true
---	

# Javascript - Optional Tracking

##Enhanced Visitor Tracking (recommended)

Jirafe will automatically parse a visit’s landing URL and referring URL to determine where the visit came from based on a list of rules. Any subsequent orders and revenue resulting from the visit will be attributed to the visit’s referring source. For some merchants, this is sufficient, makes it easy to get started and requires no additional instrumentation.

However, if you currently employ URL parameter tracking for tracking inbound marketing attribution, you can still use your tracking parameters instead of relying on Jirafe’s standard business rules. This will then be reflected in your marketing attribution reports. Here’s how to implement this.

## Implementation
Jirafe includes a section for ‘customer marketing attribution’ in the jirafe.tag file provided to you with your installation. This code will extract specific URL parameters from your pageviews and save these to the attribution property of the pageview’s metadata (the data object). If you set this property, this custom attribution will take precedence over the the default Jirafe attribution business rules. 

The following is an extract from the jirafe.tag file. There is a block of instructional comments, followed by an active line of code setting the `data.attribution` property via the `jirafe_parseAttribution` function.


```
/* Custom Marketing Attribution
You can pass in an array of url parameters to extract from the URL. Array position is important, and will be interpreted left-to-right as least specific (highest-level attribution) to most specific. 
For example...
       
       data.attribution = jirafe_parseAttribution(["atr1", "atr2", "atr3", "atr4"]);       

       ...will extract url parameters from...

       http://www.store.com?atr1=email&atr2=newsletter&atr3=2013-05&atr4=cta_button

       ...and set data.attribution to the following...

       data.attribution = ["email", "newsletter", "2013-05", "cta_button"];
*/


data.attribution = jirafe_parseAttribution(["jirafe_atr1", "jirafe_atr2", "jirafe_atr3", "jirafe_atr4", "jirafe_atr5"]);
```
Notice that the commented example looks for URL parameters of “atr1”, “atr2”, etc. whereas the active line of code looks for “jirafe_atr1”, “jirafe_atr2”, etc. This is to stress that these URL parameters are arbitrary defined by you and your marketing team.

You can set any number of levels (array length) of attribution, however Jirafe reports will only drill down to a maximum of 5 levels at this time.

Once the `data.attribution` property is set, Jirafe’s Javascript will then save this property to a browser cookie. Every subsequent event sent via the Javascript event API within the same visit will be automatically decorated with this attribution array.

After the initial pageview, the “attribution” property either should not be set or alternatively set to an empty array [ ].  In both cases we will know not to reset the attribution. More detail on this below.

##Setting The Attribution Property

a) We recommend that you call the `jirafe_parseAttribution()` function on every pageview. If you followed the implementation example above, this will happen automatically.

First pageview: <http://www.store.com?ja1=email&ja2=newsletter&ja3=2013-03>

`jirafe_parseAttribution(["ja1","ja2","ja3"])` will set the attribution property to ["email","newsletter","2013-03"]. This property will be sent with the pageview event and from this moment on, the visit will be associated with this attribution. This property will also be saved to the browser cookie and attached to every subsequent event that occurs during the visit.

Second pageview: <http://www.store.com/categories/dresses>

`jirafe_parseAttribution(["ja1","ja2","ja3"])` will set the attribution property to an empty array [ ], since no attribution keys (ja1, ja2, ...) can be found in the URL query string. If the Jirafe beacon sees a missing or empty attribution array, it knows not to reset your attribution to an empty array. All subsequent events will still be decorated with ["email","newsletter","2013-03"].

Third pageview: <http://www.store.com/products/red-sundress>

Same as for the second pageview.

b) It is very important NOT to choose custom URL attribution parameter keys that you reuse for other purposes on your store. This will cause jirafe_parseAttribution() to reset the attribution property whenever it sees those keys, which will associate subsequent events with attribution values that are nonsensical and related to internal use cases rather than attribution.

c) If some of your visits have custom attribution (url parameters exist) and some visits don’t have custom attribution (url parameters don’t exist), the visits without custom attribution will fall back on Jirafe default attribution rules. For example, direct visits to your store will generally by definition lack url parameters and will fall back on Jirafe default rules, which will assign the visit to the Direct channel.

d) If you want to implement more complicated custom attribution tracking, for example some combination of url parameters and custom URL business rules (url parsing, domain matching, etc.), please reach out to your Jirafe Professional Services contact.

##New Custom Attribution Property Begins a New Visit
In the middle of an existing visit, if Jirafe’s beacon sees a new pageview with a different non-empty attribution string, Jirafe will end the current visit and start a new visit. Here is an example.

Pageview: <http://www.store.com?ja1=email&ja2=newsletter&ja3=2013-03>

A new visit is created with attribution ["email", "newsletter", "2013-03"].

Pageview: <http://www.store.com/categories/dresses>

The first visit continues...attribution remains the same.

Pageview: <http://www.store.com?ja1=sem&ja2=adwords&ja3=canada&ja4=shoes>

The customer has left the store and come back via Adwords. A new attribution array is seen. The Jirafe beacon will end the current visit and start a new visit with attribution ["sem", "adwords", "canada", "shoes"].
