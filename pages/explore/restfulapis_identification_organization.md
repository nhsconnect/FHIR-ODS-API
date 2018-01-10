---
title: Identification | Organization
keywords: usecase, Organization
tags: [rest, fhir, identification,development]
sidebar: accessrecord_rest_sidebar
permalink: restfulapis_identification_organization.html
summary: A formally or informally recognized grouping of people or organizations formed for the purpose of achieving some form of collective action. Includes companies, institutions, corporations, departments, community groups, healthcare practice groups, etc.
---
{% include custom/search.warnbanner.html %}

{% include custom/fhir.reference.html resource="Organization" page="ODSAPI-Organization" fhirlink="[Organization](https://www.hl7.org/fhir/stu3/organization.html)" content="User Stories" userlink="" %}

## 1. Read ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Organization/[id]</div>

{% include custom/read.response.html resource="Organization" content="" %}

## 2. Search ##

<div markdown="span" class="alert alert-success" role="alert">
GET [baseUrl]/Organization?[searchParameters]</div>

Returns a bundle of all `Organization` resources that match the specified search criteria.

### 2.1. Search Parameters ###

{% include custom/search.parameters.html resource="Organization" link="https://www.hl7.org/fhir/organization.html#search)" %}

| Name | Parameter Type | Description | Path |
|------|------|-------------|------|
| `_id` |`token`|The logical id of the resource (ODS Code)|	Organization.id|
| `_lastUpdated` |`date`| To select resources based on the last time they were changed|Organization.meta.lastUpdated|
| `identifier` | `token` | The identifier for the organization (ODS Code) | Organization.identifier |
| `name` | `string` | A portion of the organization's name | Organization.name |
| `active` | `token` | Whether the organization's record is still in active use | Organization.active |
| `address-postalcode` | `string` | A postcode specified in an address | Organization.address.postalCode |
| `address-city` | `string` | A city specified in an address | Organization.address.city |
| `ods-org-role` | `token` | A role of the organization| Organization.extension('https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1').extension('role').value|
| `ods-org-primaryRole` | `token` | Whether a role of the organization is it's primary role | Organization.extension('https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1').extension('primaryrole').value|

### Additional Parameters ###

| Name | Parameter Type | Description | Allowable Content |
|------|------|-------------|------|
| `_count`|`number` | Number of results per page| Whole number |
| `_summary`|`string`| Search only: just return a count of the matching resources, without returning the actual matches |'count'|

{% include custom/search.nopat.id.html para="2.1.1." resource="Organization" content="_id"  example="RTG" example2="RTG" text1="ODS Code" text2="RTG (Derby Teaching Hospitals NHS Trust)" %}

{% include custom/search.lastupdated.html para="2.1.2." resource="Organization" content="_lastUpdated" example="gt2017-04-01" text1="organization" text2="2017-04-01" %}

The supported prefixes for this search parameter are:

| Prefix | Description |
|------|------|
|gt|greater than|

{% include custom/search.nopat.identifier.html para="2.1.3." resource="Organization" content="identifier"  example="https://fhir.nhs.uk/Id/ods-organization-code|RTG" example2="RTG" text1="ODS Code" text2="RTG (Derby Teaching Hospitals NHS Trust)" %}

{% include custom/search.nopat.string.html para="2.1.4." resource="Organization" content="name"  example="Derby Teaching Hospitals NHS Trust" text1="name" text2="Derby Teaching Hospitals NHS Trust" %}

Search expressions must:

-	Contain a minimum of 3 characters and a maximum of 100 characters
-	Only include characters (A-Z a-z 0-9 &()'+ -_ -./ : : @) 'Space'

**Begins with:**

By default, a field matches a string query if the value of the field equals or starts with the supplied parameter value, after both have been normalized by case and accent.

To search for a name that begins with "Leeds", the following search should be executed: 

```
GET https://[baseurl]/Organization?name=Leeds
```
This will return the ODS records that have an Organization name that begins with "Leeds" e.g. Leeds Chest Clinic (RQS98) and Leeds Central Ambulance Station (RX847) etc.

**Contained match:**

The `:contains` modifier returns results that include the supplied parameter value anywhere within the field being searched.

To search for a name that contains "Leeds", the following search should be executed:

```
GET https://[baseurl]/Organization?name:contains=Leeds
```
This will return the ODS records that have an Organization name that contains the word "Leeds" within its name e.g South Leeds Clinical Assessment Service (5HL18) and The North Leeds Medical Practice (B86013) etc.

**Exact match:**

The `:exact` modifier returns results that match the entire supplied parameter, including casing and accents.

{% include important.html content="Note that the search is case sensitive." %}

To search for an exact name e.g. "LEEDS TEACHING HOSPITALS NHS TRUST", the following search should be executed:

```
GET https://[baseurl]/Organization?name:exact=LEEDS TEACHING HOSPITALS NHS TRUST
```

This will return the ODS record where the Organization name is exactly "LEEDS TEACHING HOSPITALS NHS TRUST". 


{% include custom/search.nopat.active.html para="2.1.5." resource="Organization" content="active"  example="true" text1="active" text2="true" %}

An ODS record contains a status of active or inactive.

To search for an active ODS record, the following search should be executed:

```
GET https://[baseurl]/organization?active=true
```
This will return all the ODS records where the status of the organisation is active.

To search for an for an inactive ODS record, the following search should be executed:

``` 
GET https://[baseurl]/organization?active=false
```
This will return all the ODS records where the status of the organisation is inactive.

{% include custom/search.nopat.string.html para="2.1.6." resource="Organization" content="address-postalcode"  example="DE22 3NE" text1="Postcode" text2="DE22 3NE" %}

Search expressions must:

-	Contain a minimum of 2 characters
-	All characters MUST be alphanumeric

**Begins with:**

By default, a field matches a string query if the value of the field equals or starts with the supplied parameter value, after both have been normalized by case and accent. 

To search for a postcode that begins with "LS1", the following search should be executed:

```
GET https://[baseurl]/organization?address-postalcode=LS1
```

This will return all the ODS records with a postcode beginning with LS1 (All organisations with postcodes including LS1, LS10, LS11, etc.)

**Contained match:**

The `:contains` modifier returns results that include the supplied parameter value anywhere within the field being searched.

To search for a postcode that contains with "LS6 4", the following search should be executed:

```
GET https://[baseurl]/organization?address-postalcode:contains=LS6 4
```

This will return all the ODS records with a postcode containing LS6 4 anywhere in the postcode e.g. Sandfield House NH, LS6 4DZ

**Exact match:**

The `:exact` modifier returns results that match the entire supplied parameter, including casing and accents.

{% include important.html content="Note that the search is case sensitive." %}

This will return all the ODS records with a postcode

To search for an exact postcode "LS6 4JN" the following search should be executed:

```
GET https://[baseurl]/organization?address-postalcode:exact=LS6 4JN
```

This will return all the ODS records with a postcode of LS6 4JN e.g Meanwood Health Centre, LS6 4JN

{% include custom/search.nopat.string.html para="2.1.7." resource="Organization" content="address-city"  example="Derby" text1="City" text2="Derby" %}

**Begins with:**

By default, a field matches a string query if the value of the field equals or starts with the supplied parameter value, after both have been normalized by case and accent.

To search for a city that begins with "Peter", the following search should be executed: 

```
GET https://[baseurl]/Organization?address-city=Peter
```
This will return the ODS records that have a city that begins with "Peter" e.g. Peterborough, Petersfield, Peterlee etc.

**Contained match:**

The `:contains` modifier returns results that include the supplied parameter value anywhere within the field being searched.

To search for a city that contains "land", the following search should be executed:

```
GET https://[baseurl]/Organization?address-city:contains=land
```
This will return the ODS records that have a city that contains the word "land" e.g Hayling Island, Llandrindod Wells, Sunderland etc.

**Exact match:**

The `:exact` modifier returns results that match the entire supplied parameter, including casing and accents.

{% include important.html content="Note that the search is case sensitive." %}

To search for an exact name e.g. "DERBY" , the following search should be executed:

```
GET https://[baseurl]/Organization?name:exact=DERBY
```

This will return the ODS record where the city is exactly "DERBY". 

{% include custom/search.nopat.role.html para="2.1.8." resource="Organization" content="ods-org-role"  example="https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1|197" text1="system" text2="https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1" text3="code" text4="197 (NHS Trust)" example2="197" text1="system" text2="197 (NHS Trust)"%}

An ODS record contains one or many roles.

* Add role query combinations

{% include custom/search.nopat.primaryrole.html para="2.1.9." resource="Organization" content="ods-org-primaryRole"  example="true" text1="code" text2="true" %}

This search parameter MUST be used in conjunction with `ods-org-role`, it cannot be executed independently.

An ODS record contains one role with a status of primary role.

To search for an ODS record with a specified primary role '157', the following search should be executed:

```
GET https://[baseurl]/Organization?ods-org-role=https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1|157&ods-org-primaryRole=true
```
or
```
GET https://[baseurl]/Organization?ods-org-role=157&ods-org-primaryRole=true
```
This will return all the ODS records with a primary role of 157 'NON-NHS ORGANISATION'.

To search for an ODS record without a specified primary role '157', the following search should be executed:

```
GET https://[baseurl]/Organization?ods-org-role=https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1|76&ods-org-primaryRole=false
```
or
```
GET https://[baseurl]/Organization?ods-org-role=76&ods-org-primaryRole=true
```
This will return all the ODS records with a role of 76 'GP PRACTICE' which is not a primary role.

{% include custom/search.count.html para="2.1.10." resource="Organization" content="_count" example="10" text1="organization" %}

{% include custom/search.summary.html para="2.1.11." resource="Organization" content="_summary" example="count" text1="organization" %}

{% include custom/search.response.html resource="Organization" %}




