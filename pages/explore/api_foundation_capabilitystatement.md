---
title: Foundation | CapabilityStatement
keywords: foundations, fhir
tags: [rest,fhir,use_case,api,foundation,noccprofile]
sidebar: accessrecord_rest_sidebar
permalink: api_foundation_capabilitystatement.html
summary: A Capability Statement is a set of capabilities of a FHIR Server that may be used as a statement of actual server functionality or a statement of required or desired server implementation.
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" page="" fhirlink="[Capability Statement](https://www.hl7.org/fhir/capabilitystatement.html)" content="User Stories" userlink="" %}


## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/metadata</div>

The /metadata path on the root of the FHIR server will return the Capability Statement for the FHIR server:

By default the response will be returned in JSON, however XML is also supported.

For details of this interaction - see the [HL7 FHIR RESTful API](https://www.hl7.org/fhir/http.html#capabilities)

All requests SHALL contain a valid ‘Authorization’ header and SHALL contain an ‘Accept’ header with at least one of the following application/fhir+xml or application/fhir+json.

## 2. Example ##

### 2.1 Request Query ###

Retrieve the Capability Statement from the FHIR Server, the format of the response body will be xml. Replace 'baseUrl' with the actual base Url of the FHIR Server.

#### 2.1.1. cURL ####

{% include custom/embedcurl.html title="Read Server Capability Statement" command="curl -H 'Accept: application/fhir+xml' -H 'Authorization: BEARER [token]' -X GET '[baseUrl]/metadata'" %}

{% include custom/search.response.headers.html resource="Conformance"  %}

### 2.3 Response Body ###

{% include important.html content="The following draft capability statement will move as the implementation guide moves on." %}

<script src="https://gist.github.com/IOPS-DEV/a28653c2db94639b1a0ab2aafd259d2f.js"></script>


{% include important.html content="The following draft capability statement will move as the implementation guide moves on." %}


### 2.4 C# ###

{% include tip.html content="C# code snippets utilise Ewout Kramer's [fhir-net-api](https://github.com/ewoutkramer/fhir-net-api) library which is the official .NET API for HL7&reg; FHIR&reg;." %}

```csharp
var client = new FhirClient("http://[fhir_base]/");
client.PreferredFormat = ResourceFormat.Json;
var resource = client.CapabilityStatement();
FhirSerializer.SerializeResourceToXml(resource).Dump();
```
