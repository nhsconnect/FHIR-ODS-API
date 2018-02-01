---
title: Build | Organisation Data Service (ODS) Search
keywords: development
tags: [build,development]
sidebar: overview_sidebar
permalink: build_organization_search.html
summary: "How to use the FHIR ODS Lookup API to perform searches on ODS, containing examples that use different technologies to perform a search. This includes HAPI Java, C# .NET and cURL."
---

{% include custom/search.warnbanner.html %}

{% include custom/ihe.reference.html apicontent="[Organization](api_entity_organisation.html) "  odscontent="[NHS Digital ODS Offical Site](https://digital.nhs.uk/organisation-data-service)"  patterncontent="[Shared Repository](https://developer.nhs.uk/library/architecture/integration-patterns/portal/)" %}

## 1. Overview ##

{% include custom/usecase.html content="A healthcare provider wishes to locate the details of several other healthcare organisations, where the information available to enter into any search parameters differs between each organisation. The available information may be one or more of, id/identifier (ODS Code), name, address, status or role. In order to process the returned results in an efficient manner, the healthcare provider requires these to be in XML format, whilst providing paged navigation." %}


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

The ODS API Lookup can use any of the search parameters defined in the [ODS Lookup API](restfulapis_identification_organization.html). 

What we have just described is shown in the diagram below. When entering the url an ODS Lookup FHIR Query is performed and an ODS Lookup response is returned with an XML body and a HTTP response code. 

{% include image.html
max-width="200px" file="build/ods-basic-flow.png" alt="Basic Process Flow ODS Search FHIR" caption="Basic Process Flow" %}

## 3. Search Parameters ##

Examples of using different technologies to perform a search.

| Parameter | Parameter Type | Description | HAPI Java | C# .NET | cURL |
|------|------|-------------|------|------|------|
| <a href="build_organization_search.html#31-search-using-ods-code">`_id`/`identifier`</a>|`token`|Search for ODS records based on their logical or business identifier|<a href="build_organization_search.html#311-hapi-java">◘</a> | <a href="build_organization_search.html#312-c-net">◘</a> | <a href="build_organization_search.html#313-curl">◘</a> |
| <a href="build_organization_search.html#32-search-using-last-updated-date">`_lastUpdated`</a> |`date`| Search for ODS records based on their last updated date| <a href="build_organization_search.html#321-hapi-java">◘</a>| <a href="build_organization_search.html#322-c-net">◘</a>|<a href="build_organization_search.html#323-curl">◘</a>|
| <a href="build_organization_search.html#33-search-using-name">`name`</a> | `string` | Search for ODS records based on their name, including modifiers. | <a href="build_organization_search.html#331-hapi-java">◘</a>|<a href="build_organization_search.html#332-c-net">◘</a>|<a href="build_organization_search.html#333-curl">◘</a>|
| <a href="build_organization_search.html#34-search-using-active-status">`active`</a> | `token` | Search for ODS records based on their status, active or inactive. | <a href="build_organization_search.html#341-hapi-java">◘</a>|<a href="build_organization_search.html#342-c-net">◘</a>|<a href="build_organization_search.html#343-curl">◘</a>|
| <a href="build_organization_search.html#35-search-using-address-postcode">`address-postalcode`</a> | `string` | Search for ODS records based on their address postcode | <a href="build_organization_search.html#351-hapi-java">◘</a>|<a href="build_organization_search.html#352-c-net">◘</a>|<a href="build_organization_search.html#353-curl">◘</a>|
| <a href="build_organization_search.html#36-search-using-address-city">`address-city`</a> | `string` | Search for ODS records based on their address city | <a href="build_organization_search.html#361-hapi-java">◘</a>|<a href="build_organization_search.html#362-c-net">◘</a>|<a href="build_organization_search.html#363-curl">◘</a>|
| <a href="build_organization_search.html#37-search-using-organisation-role">`ods-org-role`</a> | `token` | Search for ODS records based on their role | <a href="build_organization_search.html#371-hapi-java">◘</a>|<a href="build_organization_search.html#372-c-net">◘</a>|<a href="build_organization_search.html#373-curl">◘</a>|
| <a href="restfulapis_identification_organization.html#tokenods-org-primaryRole">`ods-org-primaryRole`</a> | `token` | Search for ODS records based on their primary role | <a href="build_organization_search.html#381-hapi-java">◘</a>|<a href="build_organization_search.html#382-c-net">◘</a>|<a href="build_organization_search.html#383-curl">◘</a>|
| <a href="build_organization_search.html#39-search-and-limit-the-results-per-page">`_count`</a> | `number` | Control the number of results returned per page | <a href="build_organization_search.html#391-hapi-java">◘</a>|<a href="build_organization_search.html#392-c-net">◘</a>|<a href="build_organization_search.html#393-curl">◘</a>|
| <a href="build_organization_search.html#310-search-and-return-a-count">`_summary=count`</a> | `string` |  Return a count of the matching resources | <a href="build_organization_search.html#3101-hapi-java">◘</a>|<a href="build_organization_search.html#3102-c-net">◘</a>|<a href="build_organization_search.html#3103-curl">◘</a>|


### 3.1 Search using ODS Code ###

To search for ODS records based on their logical or business identifier.

#### 3.1.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/ac96a4e81287ccef4e5cef5fbd3cb791.js"></script>

#### 3.1.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/950319b68d8441c326e03f96c9a2db29.js"></script>

#### 3.1.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/9bdcf2c7650387a8cf41dd79472fb5ce.js"></script>


### 3.2 Search using last updated date ###

To search for ODS records based on their last updated date.

#### 3.2.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/bc094bbc9b2c3f1118e56de127f8c743.js"></script>

#### 3.2.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/948a77d0f958e04797e560fefa83595d.js"></script>

#### 3.2.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/2d5fb4ff30270d88bbeb8647715255f6.js"></script>
      
### 3.3 Search using name ###

To search for ODS records based on their name, including modifiers.

#### 3.3.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/6b880533ccfbac731de71185d68ceedc.js"></script>

#### 3.3.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/f11f3107237fda4cf8bae8a453291190.js"></script>

#### 3.3.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/f4d04199cb634eab653c7c3ce40975ec.js"></script>

### 3.4 Search using active status ###

To search for ODS records based on their status, active or inactive.

#### 3.4.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/7459f30cf434c7280d039ca3f586a9a9.js"></script>

#### 3.4.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/7228a7e31a220deb02ffe233db69c2d2.js"></script>

#### 3.4.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/b7a58bed9c62fd8e9ff371c06ece8e36.js"></script>

### 3.5 Search using address postcode ###

To search for ODS records based on their address postcode. 

#### 3.5.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/bde37f79bec20a6d21d6e593f925c3f5.js"></script>

#### 3.5.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/df56b8e369dbc6360ce82d0eb7522bd7.js"></script>

#### 3.5.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/29e62bf146e5c30b725f284880a708fb.js"></script>

### 3.6 Search using address city ###

To search for ODS records based on their address city. 

#### 3.6.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/4032679d2357c0e0ba7655688069f702.js"></script>

#### 3.6.2 C# .NET####

<script src="https://gist.github.com/IOPS-DEV/00a8a9e8b59bc58ddc9ca7042a9882da.js"></script>

#### 3.6.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/3186d87e5bbb411b84c2df89b3ee6e49.js"></script>

### 3.7 Search using Organisation role ###

To search for ODS records based on their role. 

#### 3.7.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/16dd6f6e17a42418f92e08f9aac33ea2.js"></script>

#### 3.7.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/ff70e330475c03a9a008b187c3d3943a.js"></script>

#### 3.7.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/cc653098e730b217baab1a1d2ab94822.js"></script>

### 3.8 Search using an Organisations primary role ###

To search for ODS records based on their primary role. 

#### 3.8.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/74da811c4ae61c555e1bfeec174317cf.js"></script>

#### 3.8.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/f48a44b8773fe36ba05829b660b4acf5.js"></script>

#### 3.8.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/1dced73c0da9682eb956daa8e7114051.js"></script>

### 3.9 Search and limit the results per page ###
With the scale of the ODS data, the results returned from certain queries could be extensive and require a method for browsing the details returned. It is possible to control the number of results returned using the _count parameter.

#### 3.9.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/c1a5c92a5b1e99de41d131f18ec33bca.js"></script>

#### 3.9.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/2928f39c38e11375af5e71a06773d087.js"></script>

#### 3.9.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/0f52ea7128fae9b952e6e0b2f0b46c1e.js"></script>

### 3.10 Search and return a count ###
Just return a count of the matching resources, without returning the actual matches

#### 3.10.1 HAPI Java ####

<script src="https://gist.github.com/IOPS-DEV/fb91a03dd94cfc3ac51c118faae8fd9e.js"></script>

#### 3.10.2 C# .NET ####

<script src="https://gist.github.com/IOPS-DEV/da8220638697aade1297b2c2a8e003f6.js"></script>

#### 3.10.3 cURL ####

<script src="https://gist.github.com/IOPS-DEV/bf3b525be224673c55201c7165f47dc7.js"></script>

