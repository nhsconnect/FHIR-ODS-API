---
title: API Codes
keywords: development
tags: [rest,fhir,api]
sidebar: overview_sidebar
permalink: resources_api_codes.html
summary: "Details of the API Codes used in the responses."
---
{% include custom/search.warnbanner.html %}

## 1. API Codes ##

### 1.1. 2xx Http Success ###

#### 200 OK ####
Successful Operation

### 1.2. 4xx Http Client Errors ###

#### 400 Bad Request ####

Failing to send a required query parameter will result in a 400 Bad Request response.

#### 404 Not Found ####
Requesting a resource which does not exist will result in a 404 Not Found response.

#### 406 Not Acceptable ####
Requested a media type other than JSON will result in a 406 Not Acceptable response.

### 1.3. 5xx Http Server Errors ###

#### 500 Internal Server Error ####
The server encountered an unexpected condition that prevented it from fulfilling the request.