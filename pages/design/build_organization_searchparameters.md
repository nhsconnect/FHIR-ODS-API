---
title: Design | Organisation Data Service (ODS) SearchParameters
keywords: development
tags: [design,development]
sidebar: overview_sidebar
permalink: build_organization_searchparameters.html
summary: "How to use create FHIR ODS Lookup API search parameters"
---

{% include custom/search.warnbanner.html %}

{% include custom/ihe.reference.html apicontent="[Organization](api_entity_organisation.html) "  odscontent="[NHS Digital ODS Offical Site](https://digital.nhs.uk/organisation-data-service)"  patterncontent="[Shared Repository](https://developer.nhs.uk/library/architecture/integration-patterns/portal/)" %}

## 1. Overview ##

The default Organization resource does not cater for all the required data fields that are used by ODS, and therefore requires extensions to be added to the Organization profile. When adding extensions, there will be a requirement to provide a search using the data stored in the extensions. Searching using an extension is not the same as searching using an element that forms part of the standard resource. To search using an extension we must created additional `SearchParameter` instances that provide the necessary details to allow a REST search on the added extensions.


The Organisation profile includes the following extensions:

- Extension-ODSAPI-ActivePeriod-1
- Extension-ODSAPI-DateType-1
- Extension-ODSAPI-OrganizationRole-1 (Complex)
- Extension-ODSAPI-UPRN-1

Not all extensions are equal; some extensions are complex extensions, which are an extended extension.  

### Extension-ODSAPI-OrganizationRole-1 ###

This is a complex extension that contains an additional 4 extensions:

- role
- primaryRole
- activePeriod
- status

Each complex extension will require its own SearchParameter to  

## 2. Client Organisation Extension Search ##


{% include custom/information.html content="This section is still theoretical and requires confirmation of content which may be inaccurate" %}

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
    <expression value="Organization.extension('https://fhir.nhs.uk/STU3/StructureDefinition/Extension-ODSAPI-OrganizationRole-1').extension('role').value" />
    <comparator value="eq" />
    <modifier value="exact" />
</SearchParameter>
```

{% include important.html content="ODS Lookup API utilizes complex extensions. Care should be taken when creating SearchParameters to ensure that the correct expression is defined" %}

Once the SearchParameter has been registered with the FHIR server we can search using:

```
GET http://fhir.nhs.uk/STU3/Organization?ods-org-role=197
```

Which will return the following results:

<script src="https://gist.github.com/IOPS-DEV/d20f56ec9e809413507dce01275f5a51.js"></script>

The use of role code will be vital within the ODS Lookup API to reduce the number of results returned from any search that has been initiated. The combination of role code and other criteria will help keep results down to manageable proportions.

ODS Lookup API will require a set of SearchParameter instances to provide extension search capabilities across the API. The SearchParameter instances can be downloaded from [here](https://simplifier.net/Test-FHIRODSLookupAP/~resources?category=Extension)

These will need to be used with your chosen FHIR server solution.

For more information on SearchParameter review this page [How to use create FHIR ODS Lookup API search parameters](build_organization_searchparameters.html)


### Example Searches ###


Search using an organisation role, name and town:

```
GET http://fhir.nhs.uk/STU3/Organization?ods-org-role=https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1|197&name=Freeman&address=Newcastle
```

Search for an Organisation where the role of the organisation is primary:

```
GET http://fhir.nhs.uk/STU3/Organization?ods-org-role=https://fhir.nhs.uk/STU3/ODSAPI-OrganizationRole-1|197&ods-org-primaryRole=true
```
```

