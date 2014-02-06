---
layout: page
title: Merchandising Setup
categories: Getting Started
resource: true
order: 3
---	

#Merchandising Setup

This section details how to set up your merchandising categorizations.  This allows Jirafe to provide you with powerful analytics around your merchandising categories, their products and merchandising roll-up reporting.  Follow these easy steps, and you'll have everything you need to know from the technical side for setting up your merchandising reports.


##Set-up Merchandising Categorization
Setting up your merchandising categorization is a requirement to enable Jirafe’s merchandising reports. Most merchants already have the required mapping in their internal systems; it is just a matter of passing this information to Jirafe and keeping it in sync.
Specifically, Jirafe needs to have the product -> category and category -> parent category relationships across your catalog. This will allow Jirafe’s merchandising reports to report on category-level performance.

###What Do We Mean By Merchandising Categories?
Your merchandising categories are used by your merchandisers, merchandise planners and buyers. **This is most likely NOT the same as the categories you have set up on your e-commerce site, i.e. site taxonomy.**

The merchandising categories you load into Jirafe must adhere to these rules:

* Every product has only one parent category.
* Every category has only one parent category.
* Every category can have 1 or more child products and/or categories.

###How To Load Merchandising Categories Into Jirafe
Your Jirafe account management team will upload a CSV file into our system containing your merchandising categorization mapping. The columns of the CSV are as follows:

**Column 1 -- Product Code**  
This is the product code, NOT the sku code. For example, if you are selling Nike Air Jordan Sneakers, you need to provide the product code of the Nike Air Jordan Sneakers product, NOT the Nike Air Jordan Sneakers, Size: 6, Color: Red sku.

For fashion retailers, oftentimes this will be your Style Number.

Note: This is the same product code that you pass as the “product_code” attribute for the Product and Order event API.

**Column 2 -- Full Category Path**
This is the category path the product belongs to. This path should include the parent category and all ancestor categories until reaching a top-level category.

###Example CSV File
The best way to illustrate is by example. Looking at the first data row, the product 11435 belongs to the Casual Shirts category. The Casual Shirts category rolls up to Shirts, which rolls up to Men.

Product Code/ Style # | Full Category Path |
------------ | ------------- 
11437 | Mens/Shirts/Casual Shirts |
12899 | Mens/Shirts/Casual Shirts |
11622 | Mens/Shirts/Dress Shirts |
12999 | Mens/Shirts/Dress Shirts |
13444 | Mens/Shirts/Dress Shirts |
12922 | Mens/Shirts/Dress Shirts |
29444 | Mens/Sweaters |
29414 | Mens/Sweaters |
31444 | Mens/Sweaters |
10609 | Mens/Shirts/T-Shirts |
11222 | Mens/Shirts/T-Shirts |
10744 | Mens/Shirts/T-Shirts |
38555 | Womens/Sweaters |
38222 | Womens/Sweaters |
39444 | Womens/Dresses & Skirts |
40122 | Womens/Dresses & Skirts |
38777 | Womens/Dresses & Skirts |

###Steps To Take
1. Construct the CSV file in the format listed above. You may need to query your internal systems or data warehouse. Contact your Jirafe account manager if you’d like to discuss the best approach for generating this CSV.
2. Pass this CSV to your Jirafe account manager, who will upload it to Jirafe’s system. In the future we will allow you to upload CSVs yourself in Jirafe’s admin.
3. Send an updated CSV to your Jirafe account manager on a regular basis to keep Jirafe’s reporting in sync with your merchandising categorization as you add new products, change categories, etc.

And you’re done setting up merchandising!  Any questions, reach out to our [Support](mailto:support@jirafe.com "Jirafe Support") group.  We’re always happy to help.