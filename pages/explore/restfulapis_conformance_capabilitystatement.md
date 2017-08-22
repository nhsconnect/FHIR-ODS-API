---
title: Conformance | Capability Statement
keywords: foundations, fhir
tags: [foundations,rest,fhir,use_case,development]
sidebar: accessrecord_rest_sidebar
permalink: restfulapis_conformance_capabilitystatement.html
summary: A Capability Statement documents a set of capabilities (behaviours) of a FHIR Server that may be used as a statement of actual server functionality or a statement of required or desired server implementation.
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" page="" fhirlink="[CapabilityStatement](http://hl7.org/fhir/capabilitystatement.html)" content="User Stories" userlink="" %}


## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/metadata</div>

The /metadata path on the root of the FHIR server will return the Capability Statement for the FHIR server:

Alternatively, a HTTP OPTIONS request against the root of the FHIR server will also return the Capability Statement profile:

<div markdown="span" class="alert alert-success" role="alert">
OPTIONS [baseUrl]/</div>

For details of this interaction - see the [HL7 FHIR STU3 RESTful API](http://hl7.org/fhir/http.html#capabilities)

All requests SHALL contain a valid ‘Authorization’ header and SHALL contain an ‘Accept’ header with at least one of the following application/fhir+json or application/fhir+xml.

## 2. Example ##

### 2.1 Request Query ###

Retrieve the Capability Statement from the FHIR Server, the format of the response body will be xml. Replace 'baseUrl' with the actual base Url of the FHIR Server.

#### 2.1.1. cURL ####

{% include custom/embedcurl.html title="Read Server Capability Statement" command="curl -H 'Accept: application/xml+fhir' -H 'Authorization: BEARER [token]' -X GET '[baseUrl]/metadata'" %}

{% include custom/search.response.headers.html resource="CapabilityStatement"  %}

### 2.3 Response Body ###

{% include important.html content="The following draft Capability Statement will move as the implementation guide moves on." %}

```xml

```



### 2.4 C# ###

{% include tip.html content="C# code snippets utilise Ewout Kramer's [fhir-net-api](https://github.com/ewoutkramer/fhir-net-api) library which is the official .NET API for HL7&reg; FHIR&reg;." %}

```csharp
var client = new FhirClient("http://[fhir_base]/");
client.PreferredFormat = ResourceFormat.Json;
var resource = client.CapabilityStatement();
FhirSerializer.SerializeResourceToXml(resource).Dump();
```
