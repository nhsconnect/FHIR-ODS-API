---
title: Design | Organization (ODS) Search
keywords: development
tags: [design,development]
sidebar: overview_sidebar
permalink: build_organization_search.html
summary: "How to use FHIR ODS Lookup API to perform searches on ODS"
---

{% include custom/search.warnbanner.html %}

{% include custom/ihe.reference.html apicontent="[Organization](api_entity_organisation.html) "  odscontent="[NHS Digital ODS Offical Site](https://digital.nhs.uk/organisation-data-service)"  patterncontent="[Shared Repository](https://developer.nhs.uk/library/architecture/integration-patterns/portal/)" %}

## 1. Overview ##

{% include custom/usecase.html content="A healthcare provider wishes to locate the details of several other healthcare organizations, where the information available to enter into any search parameters differers between each organization. The available information may be one or more of, Organization code, name, address, status or primary roles. In order to process the returned results in an efficient manner, the healthcare provider requires these to be in XML format, whilst providing paged navigation." %}


Within the NHS, there is a requirement to identify organizations across the Health and Social Care landscape. The Organization Data Service (ODS) provides access to the repository of publishing codes that identify these organizations and provide valuable information that can reduce administration and improve data quality. ODS provide 3 file types:

- Full files which provide a complete snapshot of all organizations.
- Quarterly amendments provide amended or new records over a three month period.
- Monthly amendments provide amended or new records over a one month period.


{% include image.html
max-width="200px" file="build/ODS-Overview.png" alt="ODS Overview"
caption="ODS Data Feeds" %}

ODS Lookup API provides an API using a [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) interface following a {% include custom/patterns.inline.html content="[resource API pattern](http://www.servicedesignpatterns.com/WebServiceAPIStyles/ResourceAPI)" %} to provide access to the Organizational Data Service (ODS) database.

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
GET https://fhir.nhs.uk/STU3/Organization?postalCode=NG10 1ZZ
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
    <system value="https://fhir.nhs.uk/Id/ods-organization-code"/>
    <value value="RR8"/>
</identifier>
```

To search for Organizations by organization code, use the following query:


```
GET https://fhir.nhs.uk/STU3/Organization?identifier=https://https://fhir.nhs.uk/Id/ods-organization-code|RR8
```

This will return all organizations with a organization code of RR8 (this may be more than one). An organization code is the main identifier within a NHS Organisation or Health Enterprise. It should be noted that codes allocated to GP practices are supplied by the NHS Prescription Service.

### 2.2 Search by Logical ID ###

Organizations stored on ODS may store the organization code as either a logical id, an identifier or as both. **TO BE CONFIRMED**

A search using a logical id will return a single record which contains the details of the requested organization. There is neither the option or requirement to add any additional parameters to a logical id search, unless searching for a FHIR history record. 

{% include warning.html content="ODS contains historic organizational records. These are not the same as FHIR history records which contain previous versions of the FHIR record" %}

To search using logical id, use the following query:

```
GET https://fhir.nhs.uk/STU3/Organization/NMV04
```

If the logical id exists, the following result is returned:

```xml
<Organization xmlns="http://hl7.org/fhir">
   <id value="NMV04"></id>
   <meta>
      <versionId value="ef284764-c821-48ec-8391-101584938bff"></versionId>
      <lastUpdated value="2017-09-07T13:50:56.373+00:00"></lastUpdated>
   </meta>
   <identifier>
      <system value="https://fhir.nhs.uk/Id/ods-organization-code"></system>
      <value value="NMV04"></value>
   </identifier>
   <active value="true"></active>
   <type>
      <coding>
         <system value="https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRecordClass-1"></system>
         <code value="2"></code>
         <display value="HSCSite"></display>
      </coding>
   </type>
   <name value="SUTTONS MANOR"></name>
   <address>
      <line value="LONDON ROAD"></line>
      <line value="STAPLEFORD TAWNEY"></line>
      <city value="ROMFORD"></city>
      <district value="ESSEX"></district>
      <postalCode value="RM4 1BF"></postalCode>
      <country value="ENGLAND"></country>
   </address>
</Organization>
```

{% include note.html content="XML has been generated from a test FHIR server and is subject to change" %}

### 2.3 Searches using multiple criteria ###

Due to the scale of the ODS database it will often be a necessity to perform searches using more than one search parameter to narrow down the results returned to the end user. 

To search for any record class 2 organization in Wigan, use the following query:

```
GET http://fhir.nhs.uk/Organization?type=2&address=Wigan
```

The following bundle is returned containing two results that match the criteria used.

```xml
<Bundle xmlns="http://hl7.org/fhir">
   <id value="4e31c618-9877-4cb2-8237-cec6756d8433"></id>
   <meta>
      <versionId value="46bd9ffa-ad31-4945-9fc8-ceedb2325d64"></versionId>
      <lastUpdated value="2017-09-08T14:37:46.798+00:00"></lastUpdated>
   </meta>
   <type value="searchset"></type>
   <total value="2"></total>
   <link>
      <relation value="self"></relation>
      <url value="http://fhir.nhs.uk/STU3/Organization?type=2&amp;address=Wigan"></url>
   </link>
   <entry>
      <fullUrl value="http://fhir.nhs.uk/STU3/Organization/ef45503b-4d00-49a1-9620-6066d981820b"></fullUrl>
      <resource>
         <Organization xmlns="http://hl7.org/fhir">
            <id value="">RJY12</id>
            <meta>
               <versionId value="19e50004-7e27-48b0-a648-6f5de104cce7"></versionId>
               <lastUpdated value="2017-09-08T14:32:34.757+00:00"></lastUpdated>
            </meta>
            <extension url="https://fhir.nhs.uk/StructureDefinition/STU3/Extension-ODSAPI-ActivePeriod-1">
               <valuePeriod>
                  <extension url="https://fhir.nhs.uk/StructureDefinition/STU3/Extension-ODSAPI-DateType-1">
                     <valueString value="Operational"></valueString>
                  </extension>
                  <start value="2001-04-01"></start>
               </valuePeriod>
            </extension>
            <extension url="https://fhir.nhs.uk/StructureDefinition/STU3/Extension-ODSAPI-OrganizationRole-1">
               <extension url="role">
                  <valueCoding>
                     <system value="https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1"></system>
                     <code value="198"></code>
                     <display value="NHS TRUST SITE"></display>
                  </valueCoding>
               </extension>
               <extension url="primaryRole">
                  <valueBoolean value="true"></valueBoolean>
               </extension>
               <extension url="activePeriod">
                  <valuePeriod>
                     <extension url="https://fhir.nhs.uk/StructureDefinition/STU3/Extension-ODSAPI-DateType-1">
                        <valueString value="Operational"></valueString>
                     </extension>
                     <start value="2009-10-01"></start>
                  </valuePeriod>
               </extension>
               <extension url="status">
                  <valueString value="Active"></valueString>
               </extension>
            </extension>
            <identifier>
               <system value="https://fhir.nhs.uk/Id/ods-organization-code"></system>
               <value value="RJY12"></value>
            </identifier>
            <active value="true"></active>
            <type>
               <coding>
                  <system value="https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRecordClass-1"></system>
                  <code value="2"></code>
                  <display value="HSCSite"></display>
               </coding>
            </type>
            <name value="CHILD &amp; FAMILY PSYCHIATRIC UNIT"></name>
            <address>
               <line value="155-157 MANCHESTER ROAD"></line>
               <line value="INCE"></line>
               <city value="WIGAN"></city>
               <district value="LANCASHIRE"></district>
               <postalCode value="WN2 2JA"></postalCode>
               <country value="ENGLAND"></country>
            </address>
         </Organization>
      </resource>
      <search>
         <mode value="match"></mode>
      </search>
   </entry>
   <entry>
      <fullUrl value="http://fhir.nhs.uk/Organization/7c82bf0a-3307-426f-9a3c-c02048c2da62"></fullUrl>
      <resource>
         <Organization xmlns="http://hl7.org/fhir">
            <id value="RRF12"></id>
            <meta>
               <versionId value="fd4d7af3-dfd5-4cff-9960-538974809290"></versionId>
               <lastUpdated value="2017-09-08T14:33:30.635+00:00"></lastUpdated>
            </meta>
            <extension url="https://fhir.nhs.uk/StructureDefinition/STU3/Extension-ODSAPI-ActivePeriod-1">
               <valuePeriod>
                  <extension url="https://fhir.nhs.uk/StructureDefinition/STU3/Extension-ODSAPI-DateType-1">
                     <valueString value="Operational"></valueString>
                  </extension>
                  <start value="2001-04-01"></start>
               </valuePeriod>
            </extension>
            <extension url="https://fhir.nhs.uk/StructureDefinition/STU3/Extension-ODSAPI-OrganizationRole-1">
               <extension url="role">
                  <valueCoding>
                     <system value="https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1"></system>
                     <code value="198"></code>
                     <display value="NHS TRUST SITE"></display>
                  </valueCoding>
               </extension>
               <extension url="primaryRole">
                  <valueBoolean value="true"></valueBoolean>
               </extension>
               <extension url="activePeriod">
                  <valuePeriod>
                     <extension url="https://fhir.nhs.uk/StructureDefinition/STU3/Extension-ODSAPI-DateType-1">
                        <valueString value="Operational"></valueString>
                     </extension>
                     <start value="2009-10-01"></start>
                  </valuePeriod>
               </extension>
               <extension url="status">
                  <valueString value="Active"></valueString>
               </extension>
            </extension>
            <identifier>
               <system value="https://fhir.nhs.uk/Id/ods-organization-code"></system>
               <value value="RRF12"></value>
            </identifier>
            <active value="true"></active>
            <type>
               <coding>
                  <system value="https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRecordClass-1"></system>
                  <code value="2"></code>
                  <display value="HSCSite"></display>
               </coding>
            </type>
            <name value="PLATT BRIDGE CLINIC"></name>
            <address>
               <line value="1 RIVINGTON AVENUE"></line>
               <line value="PLATT BRIDGE"></line>
               <city value="WIGAN"></city>
               <district value="LANCASHIRE"></district>
               <postalCode value="WN2 5NG"></postalCode>
               <country value="ENGLAND"></country>
            </address>
         </Organization>
      </resource>
      <search>
         <mode value="match"></mode>
      </search>
   </entry>
</Bundle>
```

### 2.4 Search using SearchParameter ###

The structure of the data and its data types are not an exact match to the Organization resource elements used in FHIR. To overcome this type of issue, FHIR provides a facility to extend the base resource using extensions. The FHIR ODS lookup API uses several extensions to complete the data mapping from ODS to FHIR. As with all other FHIR elements, it is possible to search on extensions, although the approach to this does differ to that previosuly discussed.

In order to search ODS using an extension, an additional resource must be created for the extension. The `SearchParameter` resource is used to define the search parameter that will be used in our url.

To search for a an organization role we must create the following SearchParameter:

```xml
TO DO
```

Once uploaded to the FHIR server, this can be used as part of our query.

```xml
GET .....TO DO
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

The first example uses the same search parameters we used earlier, we are searching for Organization with a post code of NG10 1QQ. The first couple of lines setup a Stu3 FHIR context and set the baseUrl to be `https://fhir.nhs.uk/STU3/`. The output from running this code is shown earlier in this guide.

#### Java Example 1 - ODS Search ####

<script src="https://gist.github.com/IOPS-DEV/b63b4394c201fa5b31db6f8f227b16d7.js"></script>

The second example would return the same FHIR response but this time the search is using the organization code.

#### Java Example 2 - ODS Search Organization Code ####

<script src="https://gist.github.com/IOPS-DEV/e1616bdaea112231aa9ba08a8f331f12.js"></script>

#### Java Example 3 - ODS Search Organization Role ####

<script src="https://gist.github.com/IOPS-DEV/9b27d9d46b6dd328263a8c7ca9690aa9.js"></script>

{% include warning.html content="Above code is not verified. Do not use" %}

