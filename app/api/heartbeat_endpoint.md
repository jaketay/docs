---

layout: page
title: Heartbeat Endpoint
categories: API Documentation
resource: true
description: Sending application heartbeat pings to Jirafe API
version: v2
order: 12

---

# Heartbeat Endpoint

The Jirfe API supports a Heartbeat endpoint that allows you to ping the Jirafe platform as an interval ensuring that your module is running and able to reach the Jirafe platform.  This is good practice to managing uptime and is helpful in the cases of low transaction volume.

The application should ping the endpoint every 30 minutes. Also, please note that this should not just be in a system level cron, as that will only tell us that the box is up. The payload should be sent by the plugin app. 

https://event.jirafe.com/v2/{site-id}/heartbeat [POST]

**Schema:** https://github.com/jirafe/docs/blob/master/jsonschema/v2/heartbeat.json

#### Example
```json
{
	"title": "Heartbeat Schema",
	"type": "object",
	"properties": {
		"site_id": {"type": "string"},
		"instance_id": {"type": "string"},
		"is_enabled": {"type": "bool"},
		"version": {"type": "string"},
    }
}
```
### Field Descriptions:

site_id - This is the site id for your Jirafe store.  It is unique to each merchant site.

instance_id - An ID unique per server instance running Jirafe as part of your solution. In Linux, for example, we recommend using uname.  

is_enabled - Bool as to whether or not the plugin is enabled

version - Version of the built Jirafe plugin, NOT the version of the respective commerce software platform being used
