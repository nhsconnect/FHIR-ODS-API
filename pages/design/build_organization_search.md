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

The ODS API Lookup can use any of the search parameters defined in the [ODS Lookup API](api_entity_organization.html) API. 

What we have just described is shown in the diagram below. When entering the url an ODS Lookup FHIR Query is performed and an ODS Lookup response is returned with an XML body and a HTTP response code. 

{% include image.html
max-width="200px" file="build/ods-basic-flow.png" alt="Basic Process Flow ODS Search FHIR" caption="Basic Process Flow" %}

### 2.2 Search using ODS Code ###

To search for ODS records based on their logical or business identifier.

#### 2.2.1 HAPI ####


#### 2.2.2 Java ####


#### 2.2.3 C# ####


#### 2.2.4 .NET ####

### 2.3 Search using last updated date ###

To search for ODS records based on their last updated date.

#### 2.3.1 HAPI ####


#### 2.3.2 Java ####


#### 2.3.3 C# ####


#### 2.3.4 .NET ####
      
### 2.4 Search using Organisation name ###

To search for ODS records based on their name, including modifiers.

#### 2.4.1 HAPI ####


#### 2.4.2 Java ####


#### 2.4.3 C# ####


#### 2.4.4 .NET ####

### 2.5 Search using active status ###

To search for ODS records based on their status, active or inactive.

#### 2.5.1 HAPI ####


#### 2.5.2 Java ####


#### 2.5.3 C# ####


#### 2.5.4 .NET ####

### 2.6 Search using address postcode ###

To search for ODS records based on their address postcode. 

#### 2.6.1 HAPI ####


#### 2.6.2 Java ####


#### 2.6.3 C# ####


#### 2.6.4 .NET ####

### 2.7 Search using address city ###

To search for ODS records based on their address city. 

#### 2.7.1 HAPI ####


#### 2.7.2 Java ####


#### 2.7.3 C# ####


#### 2.7.4 .NET ####

### 2.8 Search using Organisation role ###

To search for ODS records based on their role. 

#### 2.8.1 HAPI ####


#### 2.8.2 Java ####


#### 2.8.3 C# ####


#### 2.8.4 .NET ####

### 2.9 Search using an Organisations primary role ###

To search for ODS records based on their primary role. 

#### 2.9.1 HAPI ####


#### 2.9.2 Java ####


#### 2.9.3 C# ####


#### 2.9.4 .NET ####

### 2.10 Search and limit the results per page ###
With the scale of the ODS data, the results returned from certain queries could be extensive and require a method for browsing the details returned. It is possible to control the number of results returned using the _count parameter.

#### 2.10.1 HAPI ####


#### 2.10.2 Java ####


#### 2.10.3 C# ####


#### 2.10.4 .NET ####

### 2.11 Search and return a count ###
Just return a count of the matching resources, without returning the actual matches

#### 2.11.1 HAPI ####


#### 2.11.2 Java ####


#### 2.11.3 C# ####


#### 2.11.4 .NET ####



