---
title: FHIR&reg; ODS API Use Case
keywords: ODS;
tags: [foundations,use_case]
sidebar: foundations_sidebar
permalink: foundations_use_case_ods_api.html
summary: "Use case to search for an ODS Record."
---

## Prerequisites ##

To use this API, the requester:

- SHALL have gone through accreditation and received an endpoint certificate and associated ASID (Accredited System ID) for the client system.
- SHALL have either:
	- Authenticated the user using national smartcard authentication, and obtained a the UUID from the user's smartcard (and associated RBAC role from CIS), or
	- Authenticated the user using an assured local mechanism, and obtained a local user ID and role
	- And pass this user information in a JSON web token - see [Cross Organisation Audit and Provenance](integration_cross_organisation_audit_and_provenance.html) for details.
- SHALL have previously traced the patient's NHS Number using PDS or an equivalent service.

## Request Headers ##

All FHIR&reg; ODS APIs should include the below additional HTTP request headers to support audit and security requirements on the Spine:

| Header               | Value |
|----------------------|-------|
| `Ssp-TraceID`        | Client System TraceID (i.e. GUID/UUID). This is a unique ID that the client system should provide. It can be used to identify specific requests when troubleshooting issues with API calls. All calls into the service should have a unique TraceID so they can be uniquely identified later if required. |
| `Ssp-From`           | Client System ASID |
| `Ssp-To`             | The Spine ASID |
| `Ssp-InteractionID`  | |
| `Ssp-Version`  | `1` |
| `Authorization`      | This will carry the base64 encoded JSON web token required for audit - see [Cross Organisation Audit and Provenance](integration_cross_organisation_audit_and_provenance.html) for details. |

- Note: The Ssp-Version defaults to 1 if not supplied (this is currently the only version of the API). This indicates the major version of the interaction, so when new major releases of this specification are released (for example releases with breaking changes), implementers will need to specify the correct version in this header.

## Search for an ODS Record ##

Search for an ODS Record `Organization` for a specified organization using a business identifier (i.e. the ODS Code).

```http
GET [base]/Organization? to be defined
```

To Update
- The `[system]` field for the Organization Identifier SHALL be populated with the identifier system URL: `https://fhir.nhs.uk/Id/ods-organization-code`.
- The `[base]` is the URL of the Spine endpoint.
- Note: The mime-type can be specified to request either XML or JSON using another URL parameter `?_format=[mime-type]`, or a `Content-Type` HTTP header as per the [FHIR specification](https://www.hl7.org/fhir/http.html#mime-type).

### Search Response ###

To Update
Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` searchset, containing either:
	- One matching `Organization` resource that conforms to the `UPDATE with profile` profile; or
	- One `OperationOutcome` resource if the interaction is a success, however no ODS record has been found.
- Where an Organization is returned, it SHALL include the `versionId` and `fullUrl` of the current version of the `organization` resource.


```json
{
 "resourceType": "Bundle",
  "id": "6f759a10-d1b6-11e6-9598-0800200c9a66",
  "type": "searchset",
  "entry": [
    {
      "resource": {
        "resourceType": "Observation",
        "id": "76e39290-d1aa-11e6-9598-0800200c9a66",
        "meta": {
          "profile": [
            "https://fhir.nhs.uk/StructureDefinition/spine-vm-observation-1"
          ]
        },
        "status": "final",
        "code": {
          "coding": [
            {
              "system": "https://fhir.nhs.uk/fhir-observation-code-1",
              "code": "0001",
              "display": "Visitors and Migrants status observation"
            }
          ]
        },
        "subject": {
          "reference": "Patient/9900002831",
          "display": "TAYLOR, Mary (Miss)"
        },
        "effectiveDateTime": "2015-01-01T15:00:00+00:00",
        "component": [
          {
            "code": {
              "coding": [
                {
                  "system": "https://fhir.nhs.uk/spine-vm-observation-component-1",
                  "code": "BASIC_CHARGEABLE_STATUS",
                  "display": "Basic Chargeable Status"
                }
              ]
            },
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "https://fhir.nhs.uk/spine-chargeable-status-1",
                  "code": "Y",
                  "display": "Chargeable"
                }
              ]
            }
          },
          {
            "code": {
              "coding": [
                {
                  "system": "https://fhir.nhs.uk/spine-vm-observation-component-1",
                  "code": "CATEGORY_CHARGEABLE_STATUS",
                  "display": "Category Chargeable Status"
                }
              ]
            },
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "https://fhir.nhs.uk/spine-category-status-1",
                  "code": "F",
                  "display": "Chargeable non-EEA"
                }
              ]
            }
          }
        ]
      }
    }
  ]
}
```

- Additional examples are available here - [XML](Examples/Bundle-Observation.xml) and [JSON](Examples/Bundle-Observation.json)

Failure: 

- SHALL return one of the below HTTP status error codes with an `OperationOutcome` resource that conforms to the `spine-operationoutcome-1` profile if the search cannot be executed (not that there is no match).
- The below table summarises the types of error that could occur, and the HTTP response codes, along with the values to expect in the `ObservationOutcome` in the response body.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display |
|-----------|----------------|------------|--------------|-----------------|
|404|error|not-found|NO_RECORD_FOUND|No record found|
|404|error|not-found|PATIENT_NOT_FOUND|Patient not found|
|400|error|invalid|INVALID_NHS_NUMBER|Invalid NHS number|
|400|error|code-invalid|INVALID_CODE_SYSTEM|Invalid code system|
|400|error|code-invalid|INVALID_CODE_VALUE|Invalid code value|
|400|error|value|INVALID_VALUE|An input field has an invalid value for its type|
|400|error|code-invalid|INVALID_IDENTIFIER_SYSTEM|Invalid identifier system|
|400|error|code-invalid|INVALID_IDENTIFIER_VALUE|Invalid identifier value|
|422|error|invariant|CONFLICTING_VALUES|Conflicting values have been specified in different fields|
|400|error|value|INVALID_ELEMENT|Invalid element|
|401|fatal|forbidden|AUTHOR_CREDENTIALS_ERROR|Author credentials error|
|400|error|invalid|INVALID_PARAMETER|Invalid parameter|
|400|error|unknown|REQUEST_UNMATCHED|Request does not match authorisation token|
|400|error|structure|MESSAGE_NOT_WELL_FORMED|Message not well formed|
|403|fatal|forbidden|NO_PATIENT_CONSENT|Patient has not provided consent to share data|
|403|fatal|forbidden|NO_ORGANISATIONAL_CONSENT|Organisation has not provided consent to share data|
|400|error|unknown|BAD_REQUEST|Bad request|
|422|error|invalid|INVALID_RESOURCE|Invalid validation of resource|
|404|error|not-found|ORGANISATION_NOT_FOUND|Organisation not found|
|404|error|not-found|PRACTITIONER_NOT_FOUND|Practitioner not found|
|200|warning|informational|PATIENT_SENSITIVE|Patient sensitive|
|403|error|forbidden|NO_RELATIONSHIP|No legitimate relationship exists with this patient|
|422|error|invalid|FHIR_CONSTRAINT_VIOLATION|FHIR constraint violated|
|422|warning|duplicate|FLAG_ALREADY_SET|Flag value was already set|
|422|error|business-rule|INVALID_REQUEST_STATE|The request exists but is not in an appropriate state for the call to succeed|
|422|error|business-rule|INVALID_REQUEST_TYPE|The type of request is not supported by the API call|
|403|error|forbidden|ACCESS_DENIED|Access has been denied to process this request|
|403|error|forbidden|ASID_CHECK_FAILED|The sender or receiver's ASID is not authorised for this interaction|


- The error codes are defined in the [Spine Error or Warning Code ValueSet](/ValueSets/spine-error-or-warning-code-1.xml)
- Error REQUEST_UNMATCHED would occur if the NHS number being requested in the search request does not match the requested_record value in the JWT - see [Cross Organisation Audit and Provenance](integration_cross_organisation_audit_and_provenance.html) for details.

```json
{
 "resourceType": "Bundle",
  "id": "67883730-d1a8-11e6-9598-0800200c9a66",
  "type": "searchset",
  "entry": [
    {
      "resource": {
        "resourceType": "OperationOutcome",
        "id": "ff00d600-d1a6-11e6-9598-0800200c9a66",
        "meta": {
          "profile": [
            "https://fhir.nhs.uk/StructureDefinition/spine-operationoutcome-1"
          ]
        },
        "issue": [
          {
            "severity": "error",
            "code": "invalid",
            "details": {
              "coding": [
                {
                  "system": "https://fhir.nhs.uk/spine-error-or-warning-code-1",
                  "code": "INVALID_NHS_NUMBER",
                  "display": "Invalid NHS number"
                }
              ]
            },
            "diagnostics": "An invalid NHS number format has been provided in the request"
          }
        ]
      }
    }
  ]
}
```

- Additional examples are available here - [XML](Examples/Bundle-OperationOutcome.xml) and [JSON](Examples/Bundle-OperationOutcome.json)

### Example Code ###

#### C# ####

```csharp
var client = new FhirClient("http://spine-base-url/fhir-base/");
client.PreferredFormat = ResourceFormat.Json;
var query = new string[] { "identifier=https://fhir.nhs.uk/Id/nhs-number|9900002831" };
var bundle = client.Search("Patient", query);
FhirSerializer.SerializeResourceToXml(bundle).Dump();
```

#### Java ####

```java
FhirContext ctx = new FhirContext();
IGenericClient client = ctx.newRestfulGenericClient("http://spine-base-url/fhir-base/");
Bundle bundle = client.search().forResource(Observation.class)
.where(new TokenClientParam("identifier").exactly().systemAndCode("https://fhir.nhs.uk/Id/nhs-number", "9900002831"))
.encodedXml()
.execute();
```

#### cURL ####

{% include embedcurl.html title="Search for Chargeable-Status Indicator" command="curl -X GET -H 'Ssp-From: 0001' -H 'Ssp-To: 0002' -H 'Ssp-InteractionID: urn:nhs:names:services:visitorsandmigrants:fhir:rest:search:observation' -H 'Cache-Control: no-cache' -H 'Ssp-TraceID: e623b4de-f6bb-be0c-956d-c4ded0d58fc0' 'http://spine-base-url/fhir-base/Observation?identifier=https://fhir.nhs.uk/Id/nhs-number%7C9900002831'" %}

