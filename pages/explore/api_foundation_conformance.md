---
title: Foundation | Conformance
keywords: foundations, fhir
tags: [rest,fhir,use_case,api,foundation,noccprofile]
sidebar: accessrecord_rest_sidebar
permalink: api_foundation_conformance.html
summary: A capability statement is a set of capabilities of a FHIR Server that may be used as a statement of actual server functionality or a statement of required or desired server implementation.
---

{% include custom/search.warnbanner.html %}

{% include custom/fhir.referencemin.html resource="" page="" fhirlink="[Capability Statement](https://www.hl7.org/fhir/capabilitystatement.html)" content="User Stories" userlink="" %}


## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/metadata</div>

The /metadata path on the root of the FHIR server will return the Conformance statement for the FHIR server:

For details of this interaction - see the [HL7 FHIR RESTful API](https://www.hl7.org/fhir/http.html#capabilities)

All requests SHALL contain a valid ‘Authorization’ header and SHALL contain an ‘Accept’ header with at least one of the following application/json+fhir or application/xml+fhir.

## 2. Example ##

### 2.1 Request Query ###

Retrieve the Conformance statement from the FHIR Server, the format of the response body will be xml. Replace 'baseUrl' with the actual base Url of the FHIR Server.

#### 2.1.1. cURL ####

{% include custom/embedcurl.html title="Read Server Conformance Statement" command="curl -H 'Accept: application/xml+fhir' -H 'Authorization: BEARER [token]' -X GET '[baseUrl]/metadata'" %}

{% include custom/search.response.headers.html resource="Conformance"  %}

### 2.3 Response Body ###

{% include important.html content="The following draft conformance statement will move as the implementation guide moves on." %}

```xml
<Conformance xmlns="http://hl7.org/fhir">
	<version value="0.1.0-alpha.0"/>
	<name value="FHIR ODS Lookup API"/>
	<status value="draft"/>
	<experimental value="true"/>
	<publisher value="HL7 UK"/>
	<date value="2017-06-09"/>
	<description value="This server implements the ODS FHIR Lookup API"/>
	<copyright value="Copyright © 2017 HL7 UK"/>
	<fhirVersion value="3.0.1"/>
	<acceptUnknown value="both"/>
	<format value="application/xml+fhir"/>
	<format value="application/json+fhir"/>
	<profile>
		<reference value="https://fhir.hl7.org.uk/StructureDefinition/ODS-Organization-1"/>
	</profile>
	<rest>
		<mode value="server"/>
		<security>
			<cors value="true"/>
			<certificate>
				<type value="application/x-pem-file"/>
				<blob/>
			</certificate>
		</security>
		<resource>
			<type value="Organization"/>
			<profile>
				<reference value="https://fhir.hl7.org.uk/StructureDefinition/ODS-Organization-1"/>
			</profile>
			<interaction>
				<code value="read"/>
				<documentation value="Read allows clients to read the current state of the Organization resource"/>
			</interaction>
			<interaction>
				<code value="search-type"/>
				<documentation value="Search allows clients to search for the Organization resource using the specified criteria"/>
			</interaction>
			<versioning value="versioned"/>
			<readHistory value="false"/>
			<updateCreate value="false"/>
			<searchParam>
				<name value="identifier"/>
				<definition value="A Organization identifier (Practice Code, Trust Code, etc)"/>
				<type value="token"/>
				<documentation value="ODS Code (i.e. http://fhir.nhs.uk/Id/Organization|E123123)"/>
				<searchParam>
					<name value="name"/>
					<definition value="A portion of the Organization name"/>
					<type value="token"/>
				</searchParam>
			</searchParam>
		</resource>
	</rest>
</Conformance>
```

{% include important.html content="The following draft conformance statement will move as the implementation guide moves on." %}


### 2.4 C# ###

{% include tip.html content="C# code snippets utilise Ewout Kramer's [fhir-net-api](https://github.com/ewoutkramer/fhir-net-api) library which is the official .NET API for HL7&reg; FHIR&reg;." %}

```csharp
var client = new FhirClient("http://[fhir_base]/");
client.PreferredFormat = ResourceFormat.Json;
var resource = client.Conformance();
FhirSerializer.SerializeResourceToXml(resource).Dump();
```
