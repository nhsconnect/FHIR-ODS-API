---
title: Identification | Organization
keywords: usecase, Organization
tags: [rest, fhir, identification,development]
sidebar: accessrecord_rest_sidebar
permalink: restfulapis_identification_organization.html
summary: A formally or informally recognized grouping of people or organizations formed for the purpose of achieving some form of collective action. Includes companies, institutions, corporations, departments, community groups, healthcare practice groups, etc.
---
{% include custom/search.warnbanner.html %}

{% include custom/fhir.reference.html resource="Organization" page="ODSAPI-Organization" fhirlink="[Organization](https://www.hl7.org/fhir/organization.html)" content="User Stories" userlink="" %}

## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Organization/[id]</div>

{% include custom/read.response.html resource="Organization" content="" %}

## 2. Search ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Organization?[searchParameters]</div>

Fetches a bundle of all `Organization` resources for the specified search criteria.

{% include custom/search.header.html resource="Organization" %}

### 2.1. Search Parameters ###

{% include custom/search.parameters.html resource="Organization" link="https://www.hl7.org/fhir/organization.html#search)" %}

| Name | Parameter Type | Description | Path |
|------|------|-------------|------|
| `identifier` | `identifier` | Any identifier for the organization (ODS code) | 
| `name` | `string` | A portion of the name of the organization | Organization.name |
| `address-postalcode` | `string` | A postalCode specified in an address | Organization.address.postcode |
| `address-city` | `string` | A city specified in an address |Organization.address.city |
| `active` | `token` | Whether the organization's record is still in active use | Organization.active |
| `ods-org-role` | `token` | Whether the role for the organization is it's primary role | Organization.extension('https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1').extension('role').value|
| `ods-org-primaryRole` | `token` | Whether the role for the organization is it's primary role | Organization.extension('https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1').extension('primaryrole').value|

### Additional Supported Parameters

|Name | Description|
|-----|------------|
| `_id` | Search parameter _id refers to the logical id of the resource |
| `_lastUpdated` | Search parameter used to select resources based on last date changed|
| `_count` | Search result parameter used to minimise number of results per page|
| `_summary`| Search result parameter used to return a subset of the resource|


{% include custom/search.nopat.identifier.html para="2.1.1." resource="Organization" content="identifier" subtext="ODS Code" example="https://fhir.nhs.uk/Id/ods-organization-code|RTG" example2="RTG" text1="ODS Code" text2="RTG (Derby Teaching Hospitals NHS Trust)" %}

{% include custom/search.nopat.string.html para="2.1.2." resource="Organization" content="name"  example="Derby Teaching Hospitals NHS Trust" text1="name" text2="Derby Teaching Hospitals NHS Trust" %}

{% include custom/search.nopat.string.html para="2.1.3." resource="Organization" content="address-postalcode"  example="DE22%203NE" text1="Postcode" text2="DE22 3NE" %}

{% include custom/search.nopat.string.html para="2.1.4." resource="Organization" content="address-city"  example="Derby" text1="City" text2="Derby" %}

{% include custom/search.nopat.active.html para="2.1.5." resource="Organization" content="active"  example="true" text1="active" text2="true" %}

{% include custom/search.nopat.role.html para="2.1.6." resource="Organization" content="role"  example="197" text1="role" text2="197" %}

{% include custom/search.nopat.primaryrole.html para="2.1.7." resource="Organization" content="primaryrole"  example="true" text1="primaryRole" text2="true" %}

{% include custom/search.id.html para="2.1.8." resource="Organization" content="_id" example="RR8" text1="organization" %}

{% include custom/search.lastupdated.html para="2.1.9." resource="Organization" content="_lastupdated" example="gt2017-04-01" text1="organization" text2="2017-04-01" %}

{% include custom/search.count.html para="2.1.10." resource="Organization" content="_count" example="10" text1="organization" %}

{% include custom/search.summary.html para="2.1.." resource="Organization" content="_summary" example="count" text1="organization" %}

{% include custom/search.response.html resource="Organization" %}




