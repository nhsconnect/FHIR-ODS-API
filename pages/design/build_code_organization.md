---
title: Code | Organization
keywords: code, Organization
tags: [rest, fhir, code ,development]
sidebar: accessrecord_rest_sidebar
permalink: build_code_organization.html
summary: A set of organization examples that use different technologies to perform a search. This includes HAPI, Java, C#, .NET and Java.
---

## 1. Organization Search Example ##

### 3.1 Request Query ###

Return all Organization resources with a ODS Code of C81010, the format of the response body will be xml. Replace 'baseUrl' with the actual base Url of the FHIR Server.

#### 3.1.1. cURL ####

{% include custom/embedcurl.html title="Search Organization" command="curl -H 'Accept: application/fhir+xml' -H 'Authorization: BEARER [token]' -X GET  '[baseUrl]/Organization?identifier=https://fhir.nhs.uk/Id/ods-organization-code|C81010'" %}

{% include custom/search.response.headers.html resource="Organization" %}
