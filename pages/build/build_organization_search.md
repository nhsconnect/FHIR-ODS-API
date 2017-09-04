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

Most NHS trusts will typically have one central system  called the Patient Administration System (PAS) thats holds a master Patient registry of all patients who have had care with the trust. In order to reduce patient administration and improve data quality, all  other MIS within the trusts will be kept in sync with the central registry (PAS) via HL7v2 [messaging](http://www.enterpriseintegrationpatterns.com/patterns/messaging/Messaging.html). Alongside these messages will be patient encounter messages as shown in the diagram below:

{% include image.html
max-width="200px" file="IHE/Iti_pam_ip.jpg" alt="Patient Identity Feeds"
caption="ODS Data Feeds" %}

ODS Lookup API gives an API using [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) interface following a {% include custom/patterns.inline.html content="[resource API pattern](http://www.servicedesignpatterns.com/WebServiceAPIStyles/ResourceAPI)" %} to provide access to the Organizational Data Service (ODS) database.

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

The ODS API Lookup can use any of the search parameters defined in the [ODS Lookup API](api_entity_organization.html) API. For example if organization code E123456 is available, the query would be.

```
GET https://fhir.nhs.uk/STU3/StructureDefinition/organization?identifier=E123456
```

A sample response is shown below

#### XML Example 1 - Bundle Organization ####

<script src="https://gist.github.com/IOPS-DEV/ddc46233d23403e2dc0d705fac690a86.js"></script>

What we have just described is shown in the diagram below. When entered the url we did a ODS Search FHIR Query and the response is called ODS Search FHIR Query Response.

{% include image.html
max-width="200px" file="design/Basic Process Flow PDQm.jpg" alt="Basic Process Flow Patient Search FHIR" caption="Basic Process Flow" %}


#### XML Example 2 - Organization ####

<script src="https://gist.github.com/KevinMayfield/6748097c5003220759292726be05c259.js"></script>

The method for returning Practitioner is the similar and an example is shown below in section 2.2

### 2.1 identifier ###

To find a organization by ODS code we use the identifier. The earlier example contained an ODS code, the ODS c ode E123456 belongs to the system `https://fhir.nhs.uk/Id/ods-organization-code`, which is identifier for the ODS code in England.

```xml
<identifier>
    <system value="https://https://fhir.nhs.uk/Id/ods-organization-code"/>
    <value value="E123456"/>
</identifier>
```

To search for Patients by NHS number, use the following query:

```
GET [baseUrl]/Patient?identifier=[system]|[code]
```

The system is `https://fhir.nhs.uk/Id/nhs-number` and the code is `9876543210`, e.g.

```
GET [baseUrl]/Patient?identifier=https://fhir.nhs.uk/Id/nhs-number|9876543210
```

This will return all Patient resources with a NHS number of 9876543210 (this may be more than one). NHS Number may not be the main patient identifier within a NHS Organisation or Health Enterprise, this is for a number of reasons:
* Patient doesn't currently have a NHS Number (foreign visitor or from another home nation in the UK)
* Patient's NHS number hasn't been validated (and so can not be used for interoperability/communication)
* Patient has not been identified accurately.

For these reasons the trust/health organisation will use it's own primary identifier, often referred to as Hospital or District number. Organisation's will need to create their own system for the identifier, in the example Example NHS Trust have used 'https://fhir.example.nhs.uk/PAS/Patient' to indicate PAS Hospital Number. They use this with the API as shown below:

```
GET [baseUrl]/Patient?identifier=https://fhir.example.nhs.uk/PAS/Patient|123345
```

{% include note.html content="Trust or Organisation can choose to use their main identifier as the logical Id. [TODO add notes about national NHS systems using NHS Number this way.]" %}

### 2.2. Java Example ###

The examples are built using [HAPI FHIR](http://hapifhir.io/) which is an open source implementation of the HL7 FHIR specification by the University Health Network, Canada. Source code can be found on [NHSConnect GitHub](https://github.com/nhsconnect/careconnect-java-examples/tree/master/ImplementationGuideExplore)

The first example uses the same search parameters we used earlier, we are searching for patients with a surname of Kanfeld, forename of Bernie and date of birth 19/Mar/1998. The first couple of lines setup a Dstu2 FHIR context and set the baseUrl to be `http://127.0.0.1:8181/Dstu2/`. The output from running this code is shown earlier in this guide.

#### Java Example 1 - Patient Search ####

<script src="https://gist.github.com/KevinMayfield/b71d1318d5fddf5012b334c74d25f561.js"></script>

The second example would return the same FHIR response but this time the search is using the patients NHS Number.

#### Java Example 2 - Patient Search NHS Number ####

<script src="https://gist.github.com/KevinMayfield/007335b96834d986c18a778cd9b28bbd.js"></script>

As previously mentioned these queries have not returned details on the patient GP or Surgery but have provided references to them which allows us to retrieve them.

```
GET [baseUrl]/Organization/24965
```

```
GET [baseUrl]/Practitoner/24967
```

The example belows shows how this could be done using java.

#### Java Example 3 - Organization and Practitioner Search ####

<script src="https://gist.github.com/KevinMayfield/8a333a1acd31a7460ca4fd508b987156.js"></script>


<!--

This is referring to a draft NHS Digital policy

## 3. National (NHS) Patient Search ##

{% include image.html
max-width="200px" file="design/National PDQ Actor Diagram.jpg" alt="National NHS Patient Search Actor Diagram"
caption="National NHS Patient Search Actor Diagram" %}

National systems in England may use the logical Id as the business identifer. NHS in England will use the NHS Number as the logical Id.  

Currently, the only national resources this would apply to are:
•	Organisation (ODS Code)
•	Patient (NHS Number)
•	Prescription (Prescription ID)
•	Practitioner (SDS ID)


{% include image.html
max-width="200px" file="design/National Basic Process Flow PDQm.jpg" alt="National NHS Process Flow PDQ FHIR" caption="National NHS Process Flow Patient Search FHIR" %}
-->
## 3. Server(/Gateway) Patient Search  ##

<!-- This section is introducing the facade pattern. This may not sound useful but is probably the most common patterns with FHIR in the UK -->
In practice many FHIR Servers will be facades or gateways. [Facades](https://en.wikipedia.org/wiki/Facade_pattern) will provide a standardised CareConnect interface to the underlying a SQL database or provide a CareConnectAPI gateway to other patient search systems such NHS Spine Mini Services Provider (SMSP) or HL7v2 Patient queries. In both cases the CareConnect client is insulated away from the interfacing technology or technology stack.

{% include image.html
max-width="200px" file="design/Gateway PDQ Actor Diagram.jpg" alt="National NHS Patient Search Actor Diagram"
caption=" Implementing Patient Search FHIR as a gateway" %}

The {% include custom/patterns.inline.html content="[Service Connector(/Gateway) Pattern ](http://www.servicedesignpatterns.com/webserviceinfrastructures/serviceconnector)" %} can insulate client applications from complex processing which is especially useful for web applications. This would typically be done in a Trust Integration Engine or other middleware products such as Apache Camel. Advantages include:
* Insulates clients from the complexities of interoperability.
* Transforms external calls into internal formats (e.g. HL7v2 / HL7v3 XML to FHIR JSON/XML)
* Allows the different security models to work with each other (e.g. OAuth2 based environment working with FHIR API using certificate based authentication)
* Simplifies client access. API Gateway can retrieve demographics from multiple sources with a single query.


#### AngularJS Example 1 - Web App Client Search ####

<script src="https://gist.github.com/KevinMayfield/88e98c89775ba707dd7a0cf4795b395a.js"></script>

<!--
| SMSP Request | FHIR Patient Search |
|--------------|---------------------|
| getPatientDetailsByNHSNumber| `GET http://[proxyUrl]/Patient?identifier=https://fhir.nhs.uk/Id/nhs-number|[NHSNumber]&birthdate=[DateOfBirth]` |
| getPatientDetailsBySearch | `GET http://[proxyUrl]/Patient?birthdate=[DateOfBirth]&given=[Forename]&family=[Surname]&gender=[Gender]&adddress-postcode=[Postcode]` |
| getPatientDetails| `GET http://[proxyUrl]/Patient?identifier=https://fhir.nhs.uk/Id/nhs-number|[NHSNumber]&birthdate=[DateOfBirth]&given=[Forename]&family=[Surname]&gender=[Gender]&adddress-postcode=[Postcode]` |
-->

{% include image.html
max-width="200px" file="design/Gateway Process Flow PDQm.jpg" alt="Gateway Process Flow Patient Search FHIR" caption="Sample Patient Search FHIR gateway process flow" %}

#### Java Example 4 - Patient Resource from CSV line ####

Example code for building a FHIR Patient resource from a CSV file can be found on  [GitHub](https://github.com/nhsconnect/careconnect-java-examples/tree/master/ImplementationGuideExplore).
