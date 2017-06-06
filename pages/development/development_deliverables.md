---
title: Developer Cheat Sheet
keywords: development deliverables
tags: [development]
sidebar: overview_sidebar
permalink: development_deliverables.html
summary: "Developer Cheat Sheet shortcuts for the <br/>technical build of Visitors and Migrants API."
---

[TODO: Insert a picture here to show the overall process (e.g. TLS, Setting Audit headers, etc)]


Visitors and Migrants Profiles:

| Profile | Example | ValueSets | Sample Code |
| :--------- |:-----: |:-----: |
| [Spine-VM-Observation-1](https://fhir.nhs.uk/StructureDefinition/Spine-VM-Observation-1) | Observation ([json](Examples/Observation.json)/[xml](Examples/Observation.xml)) | [fhir-observation-code-1](https://fhir.nhs.uk/ValueSet/fhir-observation-code-1) <br /> [spine-vm-observation-component-1](https://fhir.nhs.uk/ValueSet/spine-vm-observation-component-1) <br /> [spine-chargeable-status-1](https://fhir.nhs.uk/ValueSet/spine-chargeable-status-1) <br /> [spine-category-status-1](https://fhir.nhs.uk/ValueSet/spine-category-status-1) | [Development Example Code - Coming Soon] |
| [Spine-OperationOutcome-1](https://fhir.nhs.uk/StructureDefinition/Spine-OperationOutcome-1) | OperationOutcome ([json](Examples/OperationOutcome.json)/[xml](Examples/OperationOutcome.xml)) | [spine-error-or-warning-code-1](https://fhir.nhs.uk/ValueSet/spine-error-or-warning-code-1) | |

Audit Profiles:

| Profile | Example | ValueSets | Sample Code |
| :--------- |:-----: |:-----: |
| [Audit-Patient-1](https://fhir.nhs.uk/StructureDefinition/Audit-Patient-1) | Patient ([json](Audit/Examples/Patient.json)/[xml](Audit/Examples/Patient.xml)) |  | [Development Example Code - Coming Soon] |
| [Audit-Device-1](https://fhir.nhs.uk/StructureDefinition/Audit-Device-1) | Device ([json](Audit/Examples/Device.json)/[xml](Audit/Examples/Device.xml)) | [device-type-codes-snct-1](https://fhir.nhs.uk/ValueSet/device-type-codes-snct-1) | |
| [Audit-Organization-1](https://fhir.nhs.uk/StructureDefinition/Audit-Organization-1) | Organisation ([json](Audit/Examples/Organization.json)/[xml](Audit/Examples/Organization.xml)) | | |
| [Audit-Practitioner-1](https://fhir.nhs.uk/StructureDefinition/Audit-Practitioner-1) | Practitioner ([json](Audit/Examples/Practitioner.json)/[xml](Audit/Examples/Practitioner.xml)) | | |

