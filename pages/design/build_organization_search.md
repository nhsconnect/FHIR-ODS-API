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

What we have just described is shown in the diagram below. When entering the url an ODS Lookup FHIR Query is performed and a ODS Lookup response is returned with an XML body and a HTTP response code. 

{% include image.html
max-width="200px" file="build/ods-basic-flow.png" alt="Basic Process Flow ODS Search FHIR" caption="Basic Process Flow" %}

### 2.2 Search using ODS Code ###

To find an organisation by ODS code we use the identifier, which stores all Organisation codes in ODS. The earlier example used a postcode to locate any records, which could return more than one result. With organisation codes being unique, searching using an identifier should in theory return only one record. 

To search for Organisations by organisation code, use the following query:


```
GET https://fhir.nhs.uk/STU3/Organization?identifier=RXX 
```

This will return all organisations with an organisation code of RXX (this may be more than one). An organisation code is the main identifier within an NHS Organisation or Health Enterprise. It should be noted that codes allocated to GP practices are supplied by the NHS Prescription Service.

<script src="https://gist.github.com/IOPS-DEV/25e3e70ac76e1dc3d0d6c6d367076d4d.js"></script>
      
### 2.3 Search using Organisation Name ###

Each organisation may be identified in ODS using its name, and ODS Lookup API is able to use this in an attempted to locate an organisation held on ODS. There are 3 variations of search available to locate an ODS record using a name.

**Searching using a name**

There may be occasions where you wish to search for an organisation where the name begins with a specific name, such as "Leeds". Search expressions must:

-	Contain a minimum of 3 characters and Maximum of 100 characters
-	Only include characters (A-Z a-z 0-9 &()'+ -_ -./ : : @) 'Space'

To search for a name that begins with "Leeds", the following search should be executed: 

```
GET https://[baseurl]/organization?name=Leeds
```
This will return all the results in a `bundle` searchset that have an organisation name that begins with "Leeds" e.g Leeds Chest Clinic (RQS98) and Leeds Central Ambulance Station (RX847)


**Searching using an exact match**

The ODS lookup API provides the functionality to search for an organisation using its name. This may be the entire organisation name, part of the name or where the name contains a defined value. 

To search for an exact name the following search should be executed:

```
GET https://[baseurl]/organization?name:exact=LEED TEACHING HOSPITALS NHS TRUST
```

This will return all the results in a `bundle` searchset where the name of the organisation is exactly "LEEDS TEACHING NHS TRUST". 

{% include important.html content="Note that the search is case sensitive." %}

**Searching using a contained match**

There may be occasions where only part of the organisation name is know, therefore an exact match cannot be found. In this scenario, a search can be performed which will look for any part of the name that contains the specified value.

To search for a name that contains "LEEDS", the following search should be executed: 

```
GET https://[baseurl]/organization?name:contains=Leeds
```

The search will return a `bundle` searchset with all organisations that contains the word "Leeds" within its name. e.g South Leeds Clinical Assessment Service (5HL18) and The North Leeds Medical Practice (B86013)

### 2.3 Searches using postcode ###

Where an organisation address contains a postcode, the ODS Lookup API is able to use this in an attempted to locate an organisation held on ODS. There are 3 variations of search available to locate an ODS record using a postcode. Search expressions must:

-	Contain a minimum of 2 characters
-	All characters MUST be alphanumeric


**Searching using a partial postcode**

Where a partial organisation postcode is available e.g LS1, it is possible to search ODS using this value. 

```
GET https://[baseurl]/organization?address-postalcode=LS1
```

This will return the results in a `bundle` searchset for any organisation with a postcode beginning with LS1 e.g. All organisations with postcodes including LS1, LS10, LS11, LS12 

**Searching using a contained match**

There may be occasions where only part of the organisation name is know, therefore an exact match cannot be found. In this scenario, a search can be performed which will look for any part of postcode that contains the specified value.

```
GET https://[baseurl]/organization?address-postalcode:contains=LS6 4
```

This will return the results in a `bundle` searchset for any organisation containing LS6 4 anywhere in the postcode e.g. Sandfield House NH, LS6 4DZ

**Searching using an exact match**

Where a full postcode for an organisation is known, this can be used to search ODS. 

To search for an exact postcode the following search should be executed:

```
GET https://[baseurl]/organization?address-postalcode:exact=LS6 4JN
```

This will return all the results in a `bundle` searchset where the postcode of the organisation is exactly LS6 4JN e.g Meanwood Health Centre, LS6 4JN

{% include important.html content="Note that the search is case sensitive." %}

### 2.4 Searches using address city###

Where an organisation address is available, the ODS Lookup API is able to use this in an attempted to locate an organisation held on ODS. There are 3 variations of search available to locate an ODS record using a city.

**Search using a partial city**

Where a partial city is known e.g Peter, it is possible to search ODS using this value.

```
GET  https://[baseurl]/organization?address-city=Peter
```

This will return all the results in a `bundle` searchset where the city begins with Peter e.g Peterborough, Petersfield, Peterlee.

**Searching using a contained match**

Where part of the city name is known, this can be used to query ODS.

```
GET https://[baseurl]/organization?address-city:contains=Land
```

This will return the results in a `bundle` searchset for any organisation containing "Land" anywhere in the city e.g. Hayling Island, Llandrindod Wells, Sunderland 

**Searching using an exact match**

Where a city for an organisation is known, this can be used to search ODS. 

To search for an exact city the following query should be executed:

```
https://[baseurl]/organization?address-city:exact=DERBY
```

This will return all the results in a `bundle` searchset where the city of the organisation is exactly Derby 

{% include important.html content="Note that the search is case sensitive." %}

### 2.5 Search using record status ###

An ODS record may have one of two statuses set;active or inactive. This translates within the ODS lookup API to active being true or false. 

```
GET https://[baseurl]/organization?active=true
```
This will return all the results in a `bundle` searchset where the status of the organisation is active.

``` 
GET https://[baseurl]/organization?active=false
```
This will return all the results in a `bundle` searchset where the status of the organisation is inactive.

### 2.6 Search for Organisation Roles ###

Organisations within ODS have a specific role, which may also be a primary role e.g NHS TRUST as a primary role. The primary role is indicated using a marker that the ODS Lookup API interprets as true or false.

**Search for a role**

|Search|REST interaction|Expected Result|Example XML|
|------|----------------|---------------------|-----------|
|Role|https://[baseurl]/Organization?ods-org-role=197|This should return all Organizations with the role of ‘NHS TRUST‘||
|Primary role|https://[baseurl]/Organization?ods-org-primaryRole=true|This should return all records with a primary role||
|Role and Primary role|https://[baseurl]/Organization?ods-org-role=197&ods-org-primaryRole=true|This should return all primary Organizations with the role of ‘NHS TRUST‘||
|Multiple roles|https://[baseurl]/Organization?ods-org-role=157&ods-org-role=15|This should return all Organizations with the roles of ‘NON-NHS ORGANISATION‘ **and** ‘REG'D UNDER PART 2 CARE STDS ACT 2000’||
||https://[baseurl]/Organization?ods-org-role=157,15|This should return all Organizations with the roles of ‘NON-NHS ORGANISATION‘ or ‘REG'D UNDER PART 2 CARE STDS ACT 2000’||


### 2.7 Searches using last updated date ###

```
GET https://[baseurl]/organization?_lastUpdated=gt2010-10-01
```
This should return all records updated since 1st Oct 2010 

### 2.8 _lastUpdated

The search parameter _lastUpdted may be used to return only records that have been updated since a specified date.

```
GET https://[baseurl]/organization?_lastUpdated=gt2010-10-01
```
This will return a `bundle` searchset containing all records updated since 1st October 2010.

### 2.9 Paging ###

With the scale of the ODS data, the results returned from certain queries could be extensive and require a method for browsing the details returned. FHIR pagination provides the ability to return paged results, making the navigating of the results user friendly. The paging function is transparent to the client, and configured via the FHIR server, however it is possible to control the number of results returned to the client using the `_count` parameter:

```
GET http://fhir.nhs.uk/STU3/Organization?type=2&city=Leeds&_count=20
```
This would limit the results to a maximum of 20 items per page.






