---
title: Foundation | CodeSystem
keywords: foundations, fhir
tags: [foundation,use_case,fhir,rest,api,noccprofile]
sidebar: accessrecord_rest_sidebar
permalink: api_foundation_codesystem.html
summary: A code system specifies a set of codes drawn from more code systems.
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" page="" fhirlink="[CodeSystem](http://www.hl7.org/fhir/codesystem.html)" content="User Stories" userlink="" %}


## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/CodeSystem/[id]</div>

{% include custom/read.response.html resource="CodeSystem" content="" %}

## 2. Example ##

### 2.1 Request Query ###

Return the CodeSystem ODS lookup API 'ODS API Organization Role'. Replace 'baseUrl' with the actual base Url of the FHIR Server.

#### 2.1.1. cURL ####

{% include custom/embedcurl.html title="Read ODS lookup API 'ODS API Organization Role CodeSystem'" command="curl -H 'Accept: application/fhir+xml' -H 'Authorization: BEARER [token]' -X GET  'https://fhir.nhs.uk/STU3/CodeSystem/ODSAPI-OrganizationRecordClass-1'" %}

{% include custom/search.response.headers.html resource="CodeSystem"  %}

### 2.3 Response Body ###

```xml
<CodeSystem>
    <url value="https://fhir.nhs.uk/STU3/CodeSystem/ODSAPI-OrganizationRecordClass-1" />
    <version value="0.0.1" />
    <name value="ODS API Organization Record Class" />
    <status value="draft" />
    <publisher value="NHS Digital" />
    <contact>
        <name value="Interoperability Team" />
        <telecom>
            <value value="interoperabilityteam@nhs.net" />
            <use value="work" />
        </telecom>
    </contact>
    <description value="A CodeSystem that identifies the organization record class." />
    <copyright value="Copyright © 2017 Health and Social Care Information Centre. NHS Digital is the trading name of the Health and Social Care Information Centre." />
    <content value="complete" />
    <concept>
        <code value="1" />
        <display value="HSCOrg" />
    </concept>
    <concept>
        <code value="2" />
        <display value="HSCSite" />
    </concept>
</CodeSystem>
```
