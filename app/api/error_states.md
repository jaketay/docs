---

layout: page
title: Error States
categories: API Documentation
resource: true
version: v2
order: 10

---

# Error states

On error a json response will be provided with `success: false`, an `error_type`, and an http response code. Below is a list of known response codes and error types:

### 400 bad_json
The json document that you submitted could not be decoded. Your document is most likely malformed.

### 400 invalid_type
The type of document you put is not accepted. The only valid types are `category`, `cart`, `customer`, `product`, `order`.

### 400 validation
The json provided is invalid. Error messages are listed with the corresponding path to the invalid field.

### 403 invalid_token
The `access_token` provided is invalid.

### 403 authorization
The `access_token` provided does not have access to the requested site.

### 503 unknown
An unexpected error occurred. A message will be provided for insight.
