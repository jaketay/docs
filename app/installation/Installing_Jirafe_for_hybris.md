---
layout: page
title: Jirafe for hybris
description:
categories: Installing Jirafe
resource: true
order: 2
---	

![logo](/img/jrf-hybris.png)


Introduction
============
This technical guide is your reference that will detail how to install the Jirafe Extension for hybris and provide valuable insight into how it integrates with the hybris platform to provide you with a new powerful world of commerce analytics!

Let’s get started!

Install Process Checklist
=========================
The following section outlines the installation checklist that you should follow to ensure you've completed all of teh steps necessary to have a successful installation of the Jirafe extension for hybris:

* Provide Jirafe with your Merchandising CSV file for product and category upload. Please refer to the [Merchandising Set-Up](/installation/Merchandising_Setup 'Merchandise CSV Documentation') documentation for additional detail.
* Ensure firewall rules are set up to allow for appropriate inbound and outbound traffic from Jirafe install for your staging & production environments. Please see [Firewall Instructions](/installation/Firewall 'Firewall Instructions')
* Receive from your Jirafe account representative your site credentials for both your staging and production environments.
* Install Jirafe Extension in Development and/or Staging build environments depending on your specific environment set-up. Testing should be completed locally by using the event logger and looking at events in the ‘jirafedata’ database table.
* Review and modify your ‘datamap’ JSON files accurately map to your data model consistent with the data that the Jirafe system requires.
* Test in your Staging Environment. (pre-production environment equivalent)
	* Test by looking at the local ‘jirafedata’ table.
	* Using your staging credentials, update your local.properties file in your staging environment which will allow data to be sent to the Jirafe platform and into your test site.
	* Work with your Jirafe account representative to \verify is correctly being sent
	 and collected in your test site.
	* Finally, schedule a “historical data fetch” to test historical retrieval.
* Install Jirafe Extension in your production environment and follow a similar process used in staging.

	```
NOTE:  Only do so once your production environment is ready and any production testing that may be taking place is complete.  This will ensure garbage data is not collected.  **Data cannot be removed once sent into the Jirafe production platform.**
	```
	* Test by looking at the local ‘jirafedata’ table.
	* Using your production credentials, update your local.properties file.
	* Work with your Jirafe account representative to verify data is correctly begin sent and collected in your test site.
	
	```
NOTE:  Jirafe Extension should remain in “disabled” state in order to your production environment is stable. Only activate when ready and extraneous data is removed from the ‘jirafedata’ table.
	```
* Schedule a “historical data fetch” to complete a full pull of your historical data.

Installation 
============
Install Instructions
--------------------
We assume you already have a version of hybris successsfully installed in your environment. It is of course a pre-requisite to installing the Jirafe for hybris extension. 

**IMPORTANT!!! If you already have Jirafe installed and are upgrading from an earlier version (versions 2.1 and 2.1.1 only), you must manually remove the eaIdx index on the 	jirafechangetracker table.**

		MySQL: drop index eaIdx_31347 on jirafechangetracker
		Oracle: drop index eaIdx_31347
		
**Do not confuse this with ceaIdx_31347 - that's the new version and will be created automatically when you do an update.**
	
	 
1. Add the path to jirafeextension into your localextensions.xml (in the hybris config folder):
	
		<extension dir=".../jirafeextension />
2. On CLI, integrate Jirafe extension in hybris runtime, within HYBRIS_HOME/bin/platform, run source ./setantenv.sh and 'ant clean all'.
3. Bring up localhost (<http://localhost:9001>) and initialize/update the hybris master system. Make sure jirafeextension is checked as an activated extension, and take care to update rather than initialize if you have existing Hybris data. You will also need to restart.
4. Access the HMC (hybris Management Console) to check the Jirafe setup in your browser with a URL like http://localhost:9001/hmc/hybris (or whatever is the URL instead of localhost:9001 to access your local hybris).
5. Check that Jirafe is working in your Web browser. Go to <http://localhost:9001/jirafeextension/>. You will login with your userid ‘jirafeuser’. Be sure that you set a password for this user in HMC and enable the user.
8. The installation will automatically inject tracking code into the frone end of your store front by leverage the jirafe.tag file.  The installation will replace the one that comes standard with the latest and greatest located at:

		</ext-template/yacceleratorstorefront/web/webroot/WEB-INF/tags/shared/analytics/jirafe.tag>
	used for front end storefront integration. 
	
__*Note: This is ONLY applicable if you are using one of the default hybris Accelerator store front templates that leverages the jirafe.tag file.  Otherwise, please reference [Using Javascript](/api/Javascript_Implementation_Guide 'Using Javascript') to integrate the front end tracking code.*__

9. OPTIONAL STEP: If you wish to use customized tags to track additional marketing attribution, feel free to edit the contents of the jirafe.tag file to incorporate the tags that your marketing team is tracking.  Please reference the “Optional Tracking” section below for further detail.
10. Activate and check cron jobs in the HMC.  See ‘Configuration & Set-up’ section below.
11. Once you’ve completed your installation process and Jirafe has notified you that it no longer requires remote access to retrieve historical data, for higher level of security, we recommend that you disable the userid ‘jirafeuser’ in your HMC.  

**IMPORTANT NOTE:**  _Do not delete, only disable, as you may need to re-enable if there is ever a case in which you require Jirafe to re-pull data or push updates._
	

Uninstall Instructions
----------------------
If you decide to uninstall Jirafe, follow these instructions:

1. Remove reference to jirafeextension in ${hybris}/config/localextensions.xml
2. Go to /opt/hybris/platform/bin and run: source ./setantenv.sh; ant clean all
3. Run update on hac 
4. Restart hybris 
5. Remove Jirafe Extension (optional) 
6. Go to hybris sql box and drop all jirafe tables (optional) 
7. Remove any optional javascript that was added to store front pages during integration.

Configuration & Set-up
======================
The Jirafe Extension utilizes a configuration properties file (local.properties) in order to set installation specific information and local settings for your installation of Jirafe.  The information in your local.properties file will require information specific to your site to set up your installation.  The indented texts below are examples of what settings requiring edit and change.  Your Jirafe Account Manager will provide your site specific information to configure your Jirafe Extension.  

##Oauth Configurations
The Oauth portion of the client configurations uses a single sign on server to ensure secure sending of end user data. Each of the below configurations specifies Jirafe environment information. The settings in the above fields will change for Jirafe event API and OAuth2 as you move from one environment (eg staging) to another (eg production).  Changing this data in your local.properties file allows users to move from testing to sending data to our production servers. 

Jirafe will issue "test" site tokens and "production" site tokens.  Test data and production data will be available for users to view in their Jirafe dashboard as different sites.  

```
# Basic configuration
# Jirafe Tag Site ID (one line per site in a multi-site environment)

jirafe.site.id=
jirafe.site.id.electronics.local=
jirafe.site.id.apparel-uk.local=

# Jirafe Beacon Event API
jirafe.api.url=

# OAuth2 authentication 
jirafe.outboundConnectionConfig.client_id=
jirafe.outboundConnectionConfig.client_secret=
jirafe.outboundConnectionConfig.access_token=

# Jirafe event api and OAuth2 service URLs
jirafe.outboundConnectionConfig.event_api_url=
jirafe.outboundConnectionConfig.auth_server_url=

# Store Media property prefix (one line per site in a multi-site environment).  Only use if NOT set in the HAC
# media.electronics.http= 
# media.apparel-uk.http=
```

IMPORTANT NOTE:  In the development stages, we recommend that you test locally and verify data is correctly being written to the jirafedata table on the hybris server.

##Activating Cron Jobs
Once you have installed Jirafe and are ready to start sending us data, there are four cron jobs to activate in the hybris Merchant Cockpit:

* The Jirafe “data sync” (every 5 minutes)
* The Jirafe “heartbeat” (every 30 minutes)
* The Jirafe “data clean up” table clean up (every 60 minutes)
* The Jirafe “catalog sync” (every 5 minutes)

###Data Sync Cron Job
The data sync cron job setting is designed to give you the flexibility to throttle how quickly data is sent to Jirafe, and as a consequence, it will impact how fast Jirafe receives and displays data for your users.  It is important to make sure it runs often so your end users can have data that is timely and up-to-date.  For this reason, we suggest setting it to run every five minutes.

###Heartbeat Configuration
The Jirafe Heartbeat is simply a health check. If Jirafe does not receive a heartbeat within a certain interval based on user activity then a member of our services team will reach out to a client contact to let them know that they are no longer sending data to Jirafe.

##Database Event Statuses
Events to be sent to Jirafe are held in a staging table.  Each record in this table has an associated status.  A cron job will periodically remove events from this table based on the status.  Which events are removed is controlled by a configuration setting: “jirafe.cronjob.cleanUp.statuses”.

The valid status codes are as follows: 

* ACCEPTED = Jirafe has received and consumed the event. (In general, you always want these objects to be cleaned.)
* REJECTED = Object is malformed. These are removed by deafult.
* NOT_AUTHORIZED = Object was sent without proper authorization. The Jirafe Plugin will retry these events, so by default this status code is NOT cleaned. 

Different statuses are useful at different stages of integration. In development, the cron job should not be run or should be set to clean up no statuses.

When the site is sending data to a Jirafe test site or when the site is in production, it is recommended that the config setting is set to ACCEPTED only.

##Visitor Tracking 
Assuming you are utilizing the accelerator and basing your implementation on an existing storefront that comes with the hybris installation, the Jirafe hybris extension is able to leverage jirafe.tags in order to access front-end events (visit related events) with minimal effort required from you.  As part of your installation process, the jirafe.tag file that comes out-of-the-box with hybris will be over-written with the latest supported version of the Jirafe.tag file.  It is located on the hybris server at: 

```
/ext-template/yacceleratorstorefront/web/webroot/WEB-INF/tags/shared/analytics/jirafe.tag   
```

This file will execute standard visit tracking events such as page views, order success confirmation, marketing attribution in addition to visitor events.

IMPORTANT NOTE:  If you are not using accelerator or able to access jirafe.tags, please notify your Jirafe Professional Services contact and they will provide you with more detailed javascript-based events documentation to help with manual integration of front-end event tracking.

#What Does the Install Do?
The Jirafe Extension for hybris does fundamental tasks to enable Jirafe on your hybris installation and begin sending data to the Jirafe analytics platform for processing:

**Installs Event Listeners** - The Jirafe plug­in utilizes event interceptors to detect event changes on key data types and changes in the hybris platform.  It by default includes listeners for:

* Products
* Orders
* Carts
* Product Categories
* Customers
* Employees (internal users)

**Installs Web Service End Points & Enables “Push”** - The Jirafe extension installs a number of secure web service endpoints and requires that push capabilities be enabled. NOTE:  It is required that a web services user be enabled so that the Jirafe platform can access end points and queries into the hybris platform.

With ‘push’ being exposed, the Jirafe platform will push new custom end points for data retrieval. Through custom end­points, the Jirafe platform will have the ability to:

* Reconfigure the plugin on necessary configurations
* Update field and data mappings to match field customization
* Fetch custom data fields if and when they are available and required

**Installs Data “Mapper”** - The extension uses a series of customizable data maps to normalize your event data to the expected Jirafe object specifications both for ongoing real time event handling and historical data retrieval.

**Installs Jirafe.tag file**  - hybris ships with a default Jirafe.tag file for front-end storefront integration.  The installation will replace with the most up-to-date version.

**Initializes “Heartbeat”** - The extension utilizes a “heartbeat” beacon to the Jirafe platform so that Jirafe’s monitoring services can ensure that your installation “alive” and functioning.  This way, Jirafe will know that the system is still operational even if the platform is not receiving data. It leverages the hybris node id and a cron job will be set on installation for every node.

#Optional Tracking
Depending on the level of visitor tracking required for your business, Jirafe has built a number of additional event and data attributes to help you track additional elements. 

If you would like to take advantage of these additional tracking capabilities, take a look at [Javascript Optional Tracking](/installation/Javascript_Optional_Tracking 'Javascript Optional Traking').

#Production Ready Checklist

If you’ve followed the installation instructions to this point, you’re ready to launch your Jirafe Extension to production.  Here’s a final checklist to make sure you have finished all the required steps.  Here's a quick list of the the things to make sure to double-check just to be sure.

1. Oauth is enabled and set to production values.  Not your test/stage authentication and site credentials.
2. All respective elements of the local.properties file are completed as outlined in the documentation.
3. All four Jirafe cron jobs are enabled in your HMC and batch sizes are set to the desired threshold.
4. The Jirafe data job (“jirafe.cronjob.cleanUp.statuses”) is set to only clean “ACCEPTED” events.
5. You see the Jirafe javascript loaded on your store front pages for visit tracking and you've completed any custom javascript to track custom variables. 
6. You've completed your historical sync of data to the Jirafe platform.
7. Check your dashboard!

You are done installing Jirafe!  

Any questions, reach out to our [Support](mailto:support@jirafe.com "Jirafe Support") group.  We’re always happy to help.

