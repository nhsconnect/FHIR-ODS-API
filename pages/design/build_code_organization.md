---
title: Code | Organization
keywords: code, Organization
tags: [rest, fhir, code ,development]
sidebar: accessrecord_rest_sidebar
permalink: build_code_organization.html
summary: A set of organization examples that use different technologies to perform a search. This includes HAPI, Java, C#, .NET and Java.
---

## 1. Organization Search Example ##

### 1.1 Request Query ###

Return all Organization resources with a ODS Code of C81010, the format of the response body will be xml. Replace 'baseUrl' with the actual base Url of the FHIR Server.

#### 1.1.1. cURL ####

{% include custom/embedcurl.html title="Search Organization" command="curl -H 'Accept: application/fhir+xml' -H 'Authorization: BEARER [token]' -X GET  '[baseUrl]/Organization?identifier=https://fhir.nhs.uk/Id/ods-organization-code|C81010'" %}

{% include custom/search.response.headers.html resource="Organization" %}

## 2.Java Examples

#### Java Example 1 - ODS Search ####

<script src="https://gist.github.com/IOPS-DEV/b63b4394c201fa5b31db6f8f227b16d7.js"></script>

The second example would return the same FHIR response but this time the search is using the organisation code.

#### Java Example 2 - ODS Search Organisation Code ####

<script src="https://gist.github.com/IOPS-DEV/e1616bdaea112231aa9ba08a8f331f12.js"></script>

#### Java Example 3 - ODS Search Organisation Role ####

<script src="https://gist.github.com/IOPS-DEV/9b27d9d46b6dd328263a8c7ca9690aa9.js"></script>

{% include warning.html content="Above code is not verified. Do not use" %}