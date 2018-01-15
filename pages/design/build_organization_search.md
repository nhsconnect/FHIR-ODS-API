---
title: Build | Organisation Data Service (ODS) Search
keywords: development
tags: [build,development]
sidebar: overview_sidebar
permalink: build_organization_search.html
summary: "How to use FHIR ODS Lookup API to perform searches on ODS containing examples that use different technologies to perform a search. This includes HAPI, Java, C# and .NET."
---

{% include custom/search.warnbanner.html %}

{% include custom/ihe.reference.html apicontent="[Organization](api_entity_organisation.html) "  odscontent="[NHS Digital ODS Offical Site](https://digital.nhs.uk/organisation-data-service)"  patterncontent="[Shared Repository](https://developer.nhs.uk/library/architecture/integration-patterns/portal/)" %}

## 1. Overview ##

{% include custom/usecase.html content="A healthcare provider wishes to locate the details of several other healthcare organisations, where the information available to enter into any search parameters differs between each organisation. The available information may be one or more of, Organisation code, name, address, status or primary roles. In order to process the returned results in an efficient manner, the healthcare provider requires these to be in XML format, whilst providing paged navigation." %}


Within the NHS, there is a requirement to identify organisations across the Health and Social Care landscape. The Organisation Data Service (ODS) provides access to the repository of publishing codes that identify these organisations and provide valuable information that can reduce administration and improve data quality. ODS provides 3 file types:

- Full files which provide a complete snapshot of all organisations.
- Quarterly amendments provide amended or new records over a three month period.
- Monthly amendments provide amended or new records over a one month period.


{% include image.html
max-width="200px" file="build/ods-overview.png" alt="ODS Overview"
caption="ODS Data Feeds" %}

ODS Lookup API provides an API using a [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) interface following a {% include custom/patterns.inline.html content="[resource API pattern](http://www.servicedesignpatterns.com/WebServiceAPIStyles/ResourceAPI)" %} to provide access to the Organisational Data Service (ODS) database.

This is particularly suited to:
* A health portal securely exposing organisational information to browser based plugins
* Medical devices which need to access organisational information
* Mobile devices used by physicians which need to establish organisational information.
* Web based EPR/EHR applications which wish to provide dynamic updates of organisation details.
* Any low resource application which exposes organisational search functionality
* A facade providing a simple API to a complex interface

## 2. Client Organisation Search ##

### 2.1 Foundation ###

{% include image.html
max-width="200px" file="build/ODS-Lookup.png" alt="Organisation Lookup FHIR Actor Diagram"
caption="Organisation Lookup FHIR Actor Diagram" %}

The ODS API Lookup can use any of the search parameters defined in the [ODS Lookup API](api_entity_organization.html). 

What we have just described is shown in the diagram below. When entering the url an ODS Lookup FHIR Query is performed and an ODS Lookup response is returned with an XML body and a HTTP response code. 

{% include image.html
max-width="200px" file="build/ods-basic-flow.png" alt="Basic Process Flow ODS Search FHIR" caption="Basic Process Flow" %}

## 3. Search Parameters ##

Examples of using different technologies to perform a search.

| Parameter | Parameter Type | Description | HAPI | Java | C# | .NET |
|------|------|-------------|------|------|------|------|
| <a href="build_organization_search.html#31-search-using-ods-code">`_id`/`identifier`</a>|`token`|Search for ODS records based on their logical or business identifier|<a href="build_organization_search.html#311-hapi">◘</a> | <a href="build_organization_search.html#312-java">◘</a> | <a href="build_organization_search.html#313-c">◘</a> | <a href="build_organization_search.html#314-net">◘</a> |
| <a href="build_organization_search.html#32-search-using-last-updated-date">`_lastUpdated`</a> |`date`| Search for ODS records based on their last updated date| <a href="build_organization_search.html#321-hapi">◘</a>|<a href="build_organization_search.html#322-java">◘</a>|<a href="build_organization_search.html#323-c">◘</a>|<a href="build_organization_search.html#324-net">◘</a>|
| <a href="build_organization_search.html#33-search-using-organisation-name">`name`</a> | `string` | Search for ODS records based on their name, including modifiers. | <a href="build_organization_search.html#331-hapi">◘</a>|<a href="build_organization_search.html#332-java">◘</a>|<a href="build_organization_search.html#333-c">◘</a>|<a href="build_organization_search.html#334-net">◘</a>|
| <a href="build_organization_search.html#34-search-using-active-status">`active`</a> | `token` | Search for ODS records based on their status, active or inactive. | <a href="build_organization_search.html#341-hapi">◘</a>|<a href="build_organization_search.html#342-java">◘</a>|<a href="build_organization_search.html#343-c">◘</a>|<a href="build_organization_search.html#344-net">◘</a>|
| <a href="build_organization_search.html#35-search-using-address-postcode">`address-postalcode`</a> | `string` | Search for ODS records based on their address postcode | <a href="build_organization_search.html#351-hapi">◘</a>|<a href="build_organization_search.html#352-java">◘</a>|<a href="build_organization_search.html#353-c">◘</a>|<a href="build_organization_search.html#354-net">◘</a>|
| <a href="build_organization_search.html#36-search-using-address-city">`address-city`</a> | `string` | Search for ODS records based on their address city | <a href="build_organization_search.html#361-hapi">◘</a>|<a href="build_organization_search.html#362-java">◘</a>|<a href="build_organization_search.html#363-c">◘</a>|<a href="build_organization_search.html#364-net">◘</a>|
| <a href="build_organization_search.html#37-search-using-organisation-role">`ods-org-role`</a> | `token` | Search for ODS records based on their role | <a href="build_organization_search.html#371-hapi">◘</a>|<a href="build_organization_search.html#372-java">◘</a>|<a href="build_organization_search.html#373-c">◘</a>|<a href="build_organization_search.html#374-net">◘</a>|
| <a href="restfulapis_identification_organization.html#tokenods-org-primaryRole">`ods-org-primaryRole`</a> | `token` | Search for ODS records based on their primary role | <a href="build_organization_search.html#381-hapi">◘</a>|<a href="build_organization_search.html#382-java">◘</a>|<a href="build_organization_search.html#383-c">◘</a>|<a href="build_organization_search.html#384-net">◘</a>|
| <a href="build_organization_search.html#39-search-and-limit-the-results-per-page">`_count`</a> | `number` | Control the number of results returned per page | <a href="build_organization_search.html#391-hapi">◘</a>|<a href="build_organization_search.html#392-java">◘</a>|<a href="build_organization_search.html#393-c">◘</a>|<a href="build_organization_search.html#394-net">◘</a>|
| <a href="build_organization_search.html#310-search-and-return-a-count">`_summary=count`</a> | `string` |  Return a count of the matching resources | <a href="build_organization_search.html#3101-hapi">◘</a>|<a href="build_organization_search.html#3102-java">◘</a>|<a href="build_organization_search.html#3103-c">◘</a>|<a href="build_organization_search.html#3104-net">◘</a>|


### 3.1 Search using ODS Code ###

To search for ODS records based on their logical or business identifier.

#### 3.1.1 HAPI ####


#### 3.1.2 Java ####


#### 3.1.3 C# ####


#### 3.1.4 .NET ####

### 3.2 Search using last updated date ###

To search for ODS records based on their last updated date.

#### 3.2.1 HAPI ####


#### 3.2.2 Java ####


#### 3.2.3 C# ####


#### 3.2.4 .NET ####
      
### 3.3 Search using Organisation name ###

To search for ODS records based on their name, including modifiers.

#### 3.3.1 HAPI ####


#### 3.3.2 Java ####


#### 3.3.3 C# ####


#### 3.3.4 .NET ####

### 3.4 Search using active status ###

To search for ODS records based on their status, active or inactive.

#### 3.4.1 HAPI ####


#### 3.4.2 Java ####


#### 3.4.3 C# ####


#### 3.4.4 .NET ####

### 3.5 Search using address postcode ###

To search for ODS records based on their address postcode. 

#### 3.5.1 HAPI ####


#### 3.5.2 Java ####


#### 3.5.3 C# ####


#### 3.5.4 .NET ####

### 3.6 Search using address city ###

To search for ODS records based on their address city. 

#### 3.6.1 HAPI ####


#### 3.6.2 Java ####


#### 3.6.3 C# ####


#### 3.6.4 .NET ####

### 3.7 Search using Organisation role ###

To search for ODS records based on their role. 

#### 3.7.1 HAPI ####


#### 3.7.2 Java ####

<script src="https://gist.github.com/IOPS-DEV/9b27d9d46b6dd328263a8c7ca9690aa9.js"></script>

#### 3.7.3 C# ####


#### 3.7.4 .NET ####

### 3.8 Search using an Organisations primary role ###

To search for ODS records based on their primary role. 

#### 3.8.1 HAPI ####


#### 3.8.2 Java ####


#### 3.8.3 C# ####


#### 3.8.4 .NET ####

### 3.9 Search and limit the results per page ###
With the scale of the ODS data, the results returned from certain queries could be extensive and require a method for browsing the details returned. It is possible to control the number of results returned using the _count parameter.

#### 3.9.1 HAPI ####


#### 3.9.2 Java ####


#### 3.9.3 C# ####


#### 3.9.4 .NET ####

### 3.10 Search and return a count ###
Just return a count of the matching resources, without returning the actual matches

#### 3.10.1 HAPI ####


#### 3.10.2 Java ####


#### 3.10.3 C# ####


#### 3.10.4 .NET ####



