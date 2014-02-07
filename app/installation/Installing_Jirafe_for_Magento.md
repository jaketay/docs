---
layout: page
title: Jirafe for Magento
categories: Installing Jirafe
resource: true
order: 3
---	

![logo]({{ site.baseurl }}/img/jrf-magento.png)


#Introduction
This technical guide is your reference that will detail how to install the Jirafe Extension for Magento available for both the Magento Community and Magento Enterprise editions.

The supported versions of Magento are Magento Community v1.7 and above, and Magento Enterprise v1.12 and above.

The guide will document how the extension integrates with the Magento platform to provide you with a new powerful world of commerce analytics.

Let’s get started!


#Install Process Checklist
* Sign up for an account with Jirafe, select Jirafe and download a copy of the Jirafe Extension for Magento. If you already have a site with us and need a copy of the extension, please contact [Jirafe Support](mailto:support@jirafe.com 'Jirafe Support').
* Provide Jirafe with your Merchandising CSV file for product and category upload. Please refer to the [Merchandising Set-Up](/installation/Merchandising_Setup.html 'Merchandise CSV Documentation') documentation for additional detail.
* If your server is installed behind a firewall, ensure firewall rules are set up to allow for appropriate outbound traffic from your server.  Your server will need to be able to access the [Jirafe API](http://event.jirafe.com) on Port 443.  For distinct IP addresses required for outbound communication, see [Firewall Instructions](/installation/Firewall.html 'Firewall Instructions').
* Ensure that cron jobs are enabled in Magento.  Jirafe utilizes cron jobs in Magento to send data to our servers.
* Ensure that Compilation Mode is disabled.  You should recompile your site and re-enable Compilation Mode after installing Jirafe.
* Ensure that mb-string is enabled on your PHP server. NOTE: This is a requirement to use Magento’s REST API.
* Install Jirafe Extension in Development and/or Staging environments first if possible using your test site credentials.  Depends on your specific environment set-up.
 

__*NOTE: You will have a different set of credentials for your test site and your production site so that your test data will not affect your production analytics.*__

* Install Jirafe Extension in Production build environment once you have verfied successfully in your test site.

__*NOTE:  Only do so once your production environment is ready to ensure garbage data is not collected.  Data cannot be removed once sent into the Jirafe production platform.*__

* If you want to track multiple sites in Jirafe, please refer to the Multi-site set-up instructions below.

__*NOTE: You need distinct site ids and credentials for each individual site you would like track.*__

* After your initial historical data sync, if the need should arise to do it again, you will need to have an authorization flag set to do so.  If you need to initiate the historical data sync again, please contact [Support](mailto:support@jirafe.com "Jirafe Support").

#Install Instructions

##Installing with Magento Connect

1. Download the Jirafe for Magento package. (Or receive a copy from your Jirafe representative).


__*NOTE: If you use Magento’s Compilation Mode, disable it before installing Jirafe by going to System > Tools > Compilation and clicking “Disable”.  You can run the compilation process and re-enable compilation mode after installing Jirafe.*__

2. Log in to the Magento Connect Manager.
In the Magento Admin, go to System > Magento Connect > Magento Connect Manager.

	![logo]({{ site-baseurl }}/img/pages/Magento_Connect_Mgr1.png)

3. In the Magento Connect Manager:
	* Turn off ‘maintenance mode’
	* Choose and upload the Jirafe package
	
	![logo]({{ site-baseurl }}/img/pages/Upload_Package.png)
	
__*NOTE:  If the Magento Connect Manager fails to refresh the cache after uploading the
		Jirafe package and you cannot access your Magento Admin, you may need to manually 		clear Magento’s cache.*__ 
		
__*This is a known problem with the Magento Connect Manager.  Please consult your systems 		engineer before performing the following commands to clear the cache:*__
			
			rm -Rf {site root file path}/var/cache
			rm -Rf {site root file path}/var/session

4.  Return to the Magento Admin and clear the Magento cache
	* Go to System > Cache Management
	* Click ‘Flush Magento Cache’ and wait for it to confirm that it is flushed  
  
	![logo]({{ site-baseurl }}/img/pages/Flush_Magento_Cache.png)
	
	* Click ‘Select All’ in the table
	* Choose ‘Refresh’ from the drop down on the right, and click ‘Submit’
	
	![logo]({{ site-baseurl }}/img/pages/Magento_Cache_Refresh.png)
	
5. Log out of the Magento Admin, and then log back in

6. Enter your Jirafe Site Credentials in the Jirafe section of your Magento Configuration for the website you want to track.  

__*NOTE: We provide you with two sets of site credentials: one for your development
environment and one for your production environment. We suggest you test Jirafe first
in your development environment with the development site credentials, and then
install Jirafe in your production environment with the production site credentials. 
You can also find your credentials in your Jirafe site settings for future reference.*__
		
* Go to System > Configuration

* Click ‘Jirafe Analytics’ in the lefthand navigation
* Choose the website you want to track from the drop down in the lefthand navigation
	
	![logo]({{ site-baseurl }}/img/pages/Choose_Site.png)
	
* Set the Enable Module drop down to “Yes” to enable Jirafe
* Enter your site credentials including Site ID, Client ID, Client Secret and OAuth Access Token
* Click ‘Save Config’
	
	![logo]({{ site-baseurl }}/img/pages/Save_Config.png)
	
* Repeat step 4: refresh the Magento cache.
* Sync your historical data with Jirafe.  This will push all of your existing data to Jirafe.  
	* On the same ‘Jirafe Analytics’ page, go back to the site you set up, and refer to the row for “Historical Data”
		
  
	![logo]({{ site-baseurl }}/img/pages/Historical_Sync.png)
	
* If you are ready to synchronize your data from your Magento system with Jirafe, click 
__*NOTE‘Sync’.  You should get a green notification that confirms that your historical sync has started.*__

__*NOTE: If you have a high volume site, we highly recommend that you wait until off-peak 		hours for your business to sync your data to ensure that you minimize load on the 		server which may impact server operations.*__
		
That’s it!  You can now log in to Jirafe.com to see analytics in your Real Time Dashboard.  Historical data could take up to a day after the sync to show up accurately in Jirafe.

##Manual Uninstall Instructions

1.  Remove Files

	```
	* app/code/community/Jirafe
	* app/code/community/Jowens
	* app/design/frontend/base/default/layout/jirafe_analytics.xml
	* app/design/frontend/base/default/template/jirafe_analytics/beacon.phtml
	* app/etc/modules/Jirafe_Analytics.xml
	* app/etc/modules/Jowens_JobQueue.xml
	* app/design/adminhtml/default/default/layout/jowens
	* app/design/adminhtml/default/default/template/jowens
	```
	
2.  Drop Tables from Database

	```
	SET FOREIGN_KEY_CHECKS = 0;
	DROP TABLE 'jirafe_analytics_batch';
	DROP TABLE 'jirafe_analytics_batch_data';
	DROP TABLE 'jirafe_analytics_data';
	DROP TABLE 'jirafe_analytics_data_attempt';
	DROP TABLE 'jirafe_analytics_data_error';
	DROP TABLE 'jirafe_analytics_data_type';
	DROP TABLE 'jirafe_analytics_log';
	DROP TABLE 'jirafe_analytics_map';
	SET FOREIGN_KEY_CHECKS = 1;
	```
	
3. Delete Core Resource

	```
	DELETE FROM 'core_config_data' WHERE 'path' LIKE 'jirafe_analytics/%'
	```
4. Delete Core Config Data
5. Clear Cache on the File System
6. Delete Admin User “Jirafe”

	```
	System > Permissions > Users > Jirafe > Delete User
	```
	
	```
	rm -RF {site root file path}/var/cache
	rm -RF {site root file path}/var/session
	rm -RF {site root file path}/var/full_page_cache (Enterprise Edition only)
	```

7. Delete Admin User Role “Jirafe”

	```
	System > Permissions > Roles > Jirafe > Delete Role
	```

8. Delete REST role 'Jirafe'

	```
	System > Web Services > REST - Roles > Jirafe > Delete Role
	```

9.  Delete Oauth Consumer "Jirafe"

	```
	System > Web Services > REST - Oauth Consumer > Jirafe > Delete
	```
	
10. Clear cache on the Magento Admin

	```
	System > Cache Management > Flush Magento Cache
	```
	
#What Does the Install Do?
The Jirafe Extension for Magento does some fundamental tasks to enable Jirafe on your Magento server to begin sending commerce data to the Jirafe analytics platform for processing:

**Installs Event Listeners** - The Jirafe plug­in utilizes ‘event listeners’ to detect event changes on key data types and changes in the Magento platform.  It includes listeners for:
  
* Products
* Orders
* Carts
* Product Categories
* Customers
* Employees (internal users)

**Installs Data Maps** - The extension uses a series of data maps to normalize your event data to the expected Jirafe object specifications both for ongoing real time event handling and historical data retrieval.

**Adds Database Tables and Sync Cron Jobs** - The extension adds database tables to store events in order to send them to Jirafe asynchronously.  It also activates a cron job which will sync events over to the Jirafe platform on an interval.  This allows for synching the data to Jirafe with a minimized load on your server.

**Installs Javascript**  - The installation will insert the required javascript into the front end store page in order to send the right visit, page, and event based data to the Jirafe platform.

**Initializes “Heartbeat”** - The extension utilizes a “heartbeat” beacon to the Jirafe platform so that Jirafe’s monitoring services can ensure that your installation “alive” and functioning.  This way, Jirafe will know that the system is still operational even if the platform is not receiving data.

**Creates User with Permissions** - The extension will create a Jirafe user with the necessary permissions and roles for the extension to operate properly, leveraging the web services framework and OAuth for security. 

#Optional Tracking

The Jirafe extension for Magento installs a javascript library, the Jirafe Beacon, on your storefront.  Depending on the level of visitor tracking required for your business, that may be enough.  The two locations where the default code exists are:

	
	./app/design/frontend/base/default/template/jirafe_analytics/beacon.phtml
	./app/code/community/Jirafe/Analytics/Block/Beacon.php
	

Jirafe also has built-in platform support for a number of additional event and data attributes to help you track additional elements.  If you would like to take advantage of these additional tracking capabilities, take a look at [Javascript Optional Tracking](Javascript_Optional_Tracking.html "Javascript Optional Traking").

#Setting up Additional Sites

If you have multiple websites within your Magento installation, Jirafe’s settings enable you to track the statistics of each website individually.  Each website in your Magento admin can be enabled or disabled from sending data to Jirafe.  This allows you to track one or many of your sites. 

The first thing you need is unique site credentials for the new site that you want to track.  In order to get a new site id and credential, take a look at [Adding Sites](Additional_Sites.html "Addtional Sites"). Once you have your new site information, do the following:

1.  Log in to your Magento Admin.
2.  Go to System > Configuration, and click on Jirafe Analytics in the left-hand navigation.
3.  From the drop down in the left-hand navigation, select the website you want to track in Jirafe.


	```
	NOTE: Magento has three levels of store management.  Jirafe works at the highest level: 	Websites.  
	If you do not know which of the options in the drop down are Websites, navigate to System 	> Manage Stores to see a list of your Websites.
	```
  
4. Enable tracking for the website. 
	* Set the Enable Module drop down to Yes.
	* Enter the Site ID, Client ID, Client Secret, and OAuth Access Token you received when you created the site.
	* Click Save Config in the upper right.
	
5. Go to System > Cache Management.
	* Flush your Magento Cache
	* Click ‘Select All’ in the table
	* Choose ‘Refresh’ from the drop down on the right, and click ‘Submit’
  
	








		
		



