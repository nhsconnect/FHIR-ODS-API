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

#### 404 Not Found ####
No record found for supplied ODS code

#### 406 Not Acceptable ####
This response can be generated for several reasons, examples are shown below:

***Invalid code system:***

Invalid ods-org-role parameter. Should be https://fhir.nhs.uk/FHIR/STU3/CodeSystem/ODSAPI-OrganizationRole-1.

***Invalid code value:***

Invalid FHIR ods-org-role parameter

***Invalid identifier system:***

Invalid ods-org-role parameter. Should be https://fhir.nhs.uk/Id/ods-organization-code

***Invalid identifier value:***

Supplied identifier must be just alphanumeric characters.

***Invalid parameter:***

Unknown argument found address-postalcode1

Unknown argument found ods-org-role1

Unknown argument found ods-org-PrimaryRole

Unknown argument found address-city1

Unknown argument found active1

***An input field has an invalid value for its type:***

Supplied address-postalcode is too short, min 2 characters

Supplied address-postalcode contains invalid characters, alphanumeric characters only

Supplied address-postalcode is too long, max 8 characters

Supplied address-city is too short, min 3 characters

Supplied date is invalid

Supplied limit is invalid, the parameter must take an integer between 0 and 20

Unknown content type specified \"application/fhir+tESTXml\"

Supplied name:contains has invalid Character/s

Supplied address-city contains invalid characters, valid characters include:  , . ' and alphabetical characters

Invalid FHIR active parameter

### 1.3. 5xx Http Server Errors ###

#### 500 Internal Server Error ####
The server encountered an unexpected condition that prevented it from fulfilling the request.