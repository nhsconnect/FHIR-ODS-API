---
title: Design | Organization (ODS) Search
keywords: development
tags: [design,development]
sidebar: overview_sidebar
permalink: build_organization_search.html
summary: "How to use FHIR ODS Lookup API to perform searches on ODS"
---

{% include custom/search.warnbanner.html %}

{% include custom/ihe.reference.html apicontent="[Organization](api_entity_organisation.html) "  ihecontent="[IHE Patient Demographic Query Mobile (IHE PDQM)](http://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_PDQm.pdf)"  patterncontent="[Shared Repository](https://developer.nhs.uk/library/architecture/integration-patterns/shared-repository/)" %}

## 1. Overview ##

{% include custom/usecase.html content="Use case to be entered here." %}



Within the NHS, there is a requirement to identify organizations across the Health and Social Care landscape. The Organizational Data Service (ODS) provides access to the repository of publishing codes that identify these organizations and provide valuable information that can reduce administration and improve data quality. ODS provide 3 file types:

- Full files which provide a complete snapshot of all organizations.
- Quarterly amendments provide amended or new records over a three month period.
- Monthly amendments provide amended or new records over a one month period.


{% include image.html
max-width="200px" file="IHE/Iti_pam_ip.jpg" alt="Patient Identity Feeds"
caption="ODS Data Feeds" %}

ODS Lookup API gives an API using [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) interface following a {% include custom/patterns.inline.html content="[resource API pattern](http://www.servicedesignpatterns.com/WebServiceAPIStyles/ResourceAPI)" %} to provide access to the Organizational Data Service (ODS) repository.

This is particularly suited to:
* A health portal securely exposing organizational information to browser based plugins
* Medical devices which need to access organizational information
* Mobile devices used by physicians which need to establish organizational information.
* Web based EPR/EHR applications which wish to provide dynamic updates of organization details.
* Any low resource application which exposes organizational search functionality
* A facade providing a simple API to a complex interface

## 2. Client Organization Search ##

### 2.1 Foundation ###

{% include image.html
max-width="200px" file="build/ODS-Lookup.png" alt="Organization Lookup FHIR Actor Diagram"
caption="Organization Lookup FHIR Actor Diagram" %}

The ODS API Lookup can use any of the search parameters defined in the [ODS Lookup API](api_entity_organization.html) API. For example if organization code RR8 is available, the query would be.

```
GET https://fhir.nhs.uk/STU3/StructureDefinition/ODSAPI-Organization-1?postalCode=NG10 1ZZ
```

A sample response is shown below

#### XML Example 1 - Bundle Organization ####

<script src="https://gist.github.com/IOPS-DEV/ddc46233d23403e2dc0d705fac690a86.js"></script>

What we have just described is shown in the diagram below. When entered the url we did a ODS Search FHIR Query and the response is called ODS Search FHIR Query Response.

{% include image.html
max-width="200px" file="build/ods-basic-flow.png" alt="Basic Process Flow ODS Search FHIR" caption="Basic Process Flow" %}


To find a organization by ODS code we use the identifier. The earlier example contained an ODS code, the ODS code RR8 belongs to the system `https://fhir.nhs.uk/Id/ods-organization-code`, which is identifier for the ODS code in England and Wales.

```xml
<identifier>
    <system value="https://https://fhir.nhs.uk/Id/ods-organization-code"/>
    <value value="RR8"/>
</identifier>
```

To search for Organizations by organization code, use the following query:


```
GET https://fhir.nhs.uk/STU3/StructureDefinition/ODSAPI-Organization-1?identifier=https://https://fhir.nhs.uk/Id/ods-organization-code|RR8
```

This will return all organizations with a organization code of RR8 (this may be more than one). An organization code is the main identifier within a NHS Organisation or Health Enterprise. It should be noted that codes allocated to GP practices are supplied by the NHS Prescription Service.

{% include note.html content="Trust or Organisation can choose to use their main identifier as the logical Id. [TODO - check accuracy of statement]" %}

### 2.2. Java Example ###

The examples are built using [HAPI FHIR](http://hapifhir.io/) which is an open source implementation of the HL7 FHIR specification by the University Health Network, Canada. Source code can be found on [NHSConnect GitHub](https://github.com/nhsconnect/careconnect-java-examples/tree/master/ImplementationGuideExplore)

The first example uses the same search parameters we used earlier, we are searching for Organization with a post code of NG10 1QQ. The first couple of lines setup a Stu3 FHIR context and set the baseUrl to be `https://fhir.nhs.uk/STU3/`. The output from running this code is shown earlier in this guide.

#### Java Example 1 - ODS Search ####

<script src="https://gist.github.com/IOPS-DEV/b63b4394c201fa5b31db6f8f227b16d7.js"></script>

The second example would return the same FHIR response but this time the search is using the organization code.

#### Java Example 2 - ODS Search Organization Code ####

<script src="https://gist.github.com/IOPS-DEV/e1616bdaea112231aa9ba08a8f331f12.js"></script>





