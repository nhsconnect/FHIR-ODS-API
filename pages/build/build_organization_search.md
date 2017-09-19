---
title: Design | Organisation Data Service (ODS) Search
keywords: development
tags: [design,development]
sidebar: overview_sidebar
permalink: build_organization_search.html
summary: "How to use FHIR ODS Lookup API to perform searches on ODS"
---

{% include custom/search.warnbanner.html %}

{% include custom/ihe.reference.html apicontent="[Organization](api_entity_organisation.html) "  odscontent="[NHS Digital ODS Offical Site](https://digital.nhs.uk/organisation-data-service)"  patterncontent="[Shared Repository](https://developer.nhs.uk/library/architecture/integration-patterns/portal/)" %}

## 1. Overview ##

{% include custom/usecase.html content="A healthcare provider wishes to locate the details of several other healthcare organisations, where the information available to enter into any search parameters differers between each organisation. The available information may be one or more of, Organisation code, name, address, status or primary roles. In order to process the returned results in an efficient manner, the healthcare provider requires these to be in XML format, whilst providing paged navigation." %}


Within the NHS, there is a requirement to identify organisations across the Health and Social Care landscape. The Organisation Data Service (ODS) provides access to the repository of publishing codes that identify these organisations and provide valuable information that can reduce administration and improve data quality. ODS provide 3 file types:

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

The ODS API Lookup can use any of the search parameters defined in the [ODS Lookup API](api_entity_organization.html) API. For example, to locate organisations with a post code of LS9 7TF, the query would be.

```
GET https://fhir.nhs.uk/STU3/Organization?postalCode=LS9 7TF
```

A sample response is shown below

#### XML Example 1 - Bundle Organisation ####

<script src="https://gist.github.com/IOPS-DEV/ddc46233d23403e2dc0d705fac690a86.js"></script>

What we have just described is shown in the diagram below. When entereing the url we did an ODS Search FHIR Query and the response is called ODS Search FHIR Query Response.

{% include image.html
max-width="200px" file="build/ods-basic-flow.png" alt="Basic Process Flow ODS Search FHIR" caption="Basic Process Flow" %}

### 2.2 Search using Identifier ###

To find a organisation by ODS code we use the identifier, which stores all Organisation codes in ODS. The earlier example used a post code to locate any records, which could return more than one result. With organisation codes being unique, searching using an identifier should in theory return only once record. The exception to this rule would be searching for records in FHIR using the `_history` parameter. 


To search for Organisations by organisation code, use the following query:


```
GET https://fhir.nhs.uk/STU3/Organization?identifier=RXX 
```

This will return all organisations with a organisation code of RXX (this may be more than one). An organisation code is the main identifier within a NHS Organisation or Health Enterprise. It should be noted that codes allocated to GP practices are supplied by the NHS Prescription Service.

<script src="https://gist.github.com/IOPS-DEV/25e3e70ac76e1dc3d0d6c6d367076d4d.js"></script>
      

### 2.2 Search by Logical ID ###

Organisations stored on ODS may store the organisation code as either a logical id, an identifier or as both. **TO BE CONFIRMED**

A search using a logical id will return a single record which contains the details of the requested organisation. There is neither the option or requirement to add any additional parameters to a logical id search, unless searching for a FHIR history record. 

{% include warning.html content="ODS contains historic organisational records. These are not the same as FHIR history records which contain previous versions of the FHIR record" %}

To search using logical id, use the following query:

```
GET https://fhir.nhs.uk/STU3/Organization/RXX
```

If the logical id exists, the following result is returned:

<script src="https://gist.github.com/IOPS-DEV/eee16fbeeee41bb5622d3964bf9a1b74.js"></script>


{% include note.html content="XML has been generated from a test FHIR server and is subject to change" %}

### 2.3 Searches using multiple criteria ###

Due to the scale of ODS it will often be a necessary to perform searches using more than one search parameter to narrow down the results returned to the end user. 

To search for organisations with a Record Class of ‘2 - HSCSite’ in Wigan, use the following query:

```
GET http://fhir.nhs.uk/Organization?type=2&address=Wigan
```

The following bundle is returned containing two results that match the criteria used.


<script src="https://gist.github.com/IOPS-DEV/ca6d412ce2363a95bce679ca3f54bcdd.js"></script>

Using the same approach, we can narrow a search down to a organization name and area. Using only a name search of 'Freeman' for example would result in a large number of results returned, covering all organization roles and locations. Adding an address within the criteria can significantly reduce the results:

```
GET http://fhir.nhs.uk/Organization?name=Freeman&address=Newcastle
```
Note that we have no specified a part of the address, such as a Town, or street. We only need to enter the address and our search will check against all child elements that are used to construct a full address.


### 2.4 Search using SearchParameter ###

{% include custom/information.html content="This section is still thorectical and requires confirmation of content which may be inaccurate" %}

The structure of the data and its data types are not an exact match to the Organisation resource elements used in FHIR. To overcome this type of issue, FHIR provides a facility to extend the base resource using extensions. The FHIR ODS lookup API uses several extensions to complete the data mapping from ODS to FHIR. As with all other FHIR elements, it is possible to search on extensions, although the approach to this does differ to that previously discussed.

In order to search ODS using an extension, an additional resource must be created for the extension. The `SearchParameter` resource is used to define the search parameter that will be used in our url.

To search for a an organisation role we must create the following SearchParameter:

```xml
<SearchParameter>
    <url value="https://fhir.nhs.uk/STU3/SearchParameter/ODSAPI-OrganizationRole-Role-1" />
    <name value="ODS API Organization Role" />
    <status value="draft" />
    <experimental value="true" />
    <date value="2017-09-12" />
    <publisher value="NHS Digital" />
    <purpose value="This search parameter has been defined to enable the ability to query on the role of an ODS Organization." />
    <code value="ods-org-role" />
    <base value="Organization" />
    <type value="token" />
    <description value="A search parameter to query on the role of an ODS Organization." />
    <expression value="Organization.extension('https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1').extension('role')" />
    <xpath value="f:Organization/f:extension[@url='https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1']/f:extension[@url='role']" />
    <xpathUsage value="normal" />
    <comparator value="eq" />
    <modifier value="exact" />
</SearchParameter>
```

{% include important.html content="ODS Lookup API utilizes complex extensions. Care should be taken when creating SearchParameters to ensure that the correct expression is defined" %}

Once the SearchParameter has been registered with the FHIR server we can search using:

```
GET http://fhir.nhs.uk/STU3/Organization?ods-org-role=https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1|197
```

Which will return the following results:

<script src="https://gist.github.com/IOPS-DEV/d20f56ec9e809413507dce01275f5a51.js"></script>ts here

The use of role code will be vital within the ODS Lookup API to reduce the number of results returned from any search that has been initiated. The combination of role code and other criteria will help keep results down to manageable proportions.

Search using an organisation role, name and town:

```
GET GET http://fhir.nhs.uk/STU3/Organization?ods-org-role=https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1|197&name=Frreeman&address=Newcastle
```

### 2.5 Paging ###

With the scale of the ODS data, the results returned from certain queries could be extensive and require a method for browsing the details returned. FHIR pagination provides the ability to return paged results, making the navigating of the results user friendly. The paging function is transparent to the client, and configured via the FHIR server, however it is possible to control the number of results returned to the client using the `_count` parameter:

```
GET http://fhir.nhs.uk/STU3/Organization?type=2&address=Wigan&_count=20
```

This would limit the results to a maximum of 20 items per page. **Check above GET statement**

TODO - More on pagination.

### 2.2. Java Example ###

The examples are built using [HAPI FHIR](http://hapifhir.io/) which is an open source implementation of the HL7 FHIR specification by the University Health Network, Canada. Source code can be found on [NHSConnect GitHub](https://github.com/nhsconnect/careconnect-java-examples/tree/master/ImplementationGuideExplore)

The first example uses the same search parameters we used earlier, we are searching for Organisation with a post code of NG10 1QQ. The first couple of lines setup a Stu3 FHIR context and set the baseUrl to be `https://fhir.nhs.uk/STU3/`. The output from running this code is shown earlier in this guide.

#### Java Example 1 - ODS Search ####

<script src="https://gist.github.com/IOPS-DEV/b63b4394c201fa5b31db6f8f227b16d7.js"></script>

The second example would return the same FHIR response but this time the search is using the organisation code.

#### Java Example 2 - ODS Search Organisation Code ####

<script src="https://gist.github.com/IOPS-DEV/e1616bdaea112231aa9ba08a8f331f12.js"></script>

#### Java Example 3 - ODS Search Organisation Role ####

<script src="https://gist.github.com/IOPS-DEV/9b27d9d46b6dd328263a8c7ca9690aa9.js"></script>

{% include warning.html content="Above code is not verified. Do not use" %}

