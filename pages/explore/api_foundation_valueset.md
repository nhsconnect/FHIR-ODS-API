---
title: Foundation | ValueSet
keywords: foundations, fhir
tags: [foundation,use_case,fhir,rest,api,noccprofile]
sidebar: overview_sidebar
permalink: api_foundation_valueset.html
summary: A ValueSet selects a set of codes from those defined by one or more code systems.
---

{% include custom/fhir.referencemin.html resource="[ODS API Organization Role](https://fhir.nhs.uk/STU3/ValueSet/ODSAPI-OrganizationRole-1)" resource1="[ODS API Organization Record Class](https://fhir.nhs.uk/STU3/ValueSet/ODSAPI-OrganizationRecordClass-1)" page="" fhirlink="[ValueSet](http://www.hl7.org/fhir/stu3/valueset.html)" content="User Stories" userlink="" %}


## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/ValueSet/[id]</div>

{% include custom/read.response.html resource="ValueSet" content="" %}

## 2. Example ##

### 2.1 Request Query ###

Return the ValueSet ODS lookup API 'ODS API Organization Role'. Replace 'baseUrl' with the actual base Url of the FHIR Server.

#### 2.1.1. cURL ####

{% include custom/embedcurl.html title="Read ODS lookup API 'ODS API Organization Role ValueSet'" command="curl -H 'Accept: application/fhir+xml' -X GET  'https://fhir.nhs.uk/STU3/ValueSet/ODSAPI-OrganizationRole-1'" %}

{% include custom/search.response.headers.html resource="ValueSet"  %}

### 2.3 Response Body ###

<script src="https://gist.github.com/IOPS-DEV/b28db7655f3a4fdf921f249bedc316cc.js"></script>
