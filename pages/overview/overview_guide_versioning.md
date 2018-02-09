---
title: Guide Versioning
keywords: development, versioning
tags: [development]
sidebar: overview_sidebar
permalink: overview_guide_versioning.html
summary: An overview of how this Implementation Guide is versioned
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the FHIR&reg; ODS Lookup API. This project is being developed using an agile methodology so iterative updates to content will be added on a regular basis." %}

## 1. Product Versioning ##

### 1.1.0 Semantic Versioning ###
Versioning of each technical “Product” or asset (i.e. API, Design Principle(s), Data Library) is managed using [Semantic Versioning 2.0.0](http://semver.org/).


Given a version number MAJOR.MINOR.PATCH, increment the:

- MAJOR version when you make incompatible API changes,
- MINOR version when you add functionality in a backwards-compatible manner, and
- PATCH version when you make backwards-compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

A pre-release version MAY be denoted by appending a hyphen and a series of dot separated identifiers immediately following patch version. Refer to [Semantic Versioning - Item 9](http://semver.org/#spec-item-9){:target="_blank"}) 

For examples: 1.0.0-alpha.1 is a valid pre-release version.

### 1.2.0 Pre-release Labels ###

When FHIR API implementation guides are published, they MUST have an associated maturity label. These labels are based on the GDS development process stages and MUST conform to one of the labels defined in the [FHIR-PUB-04: FHIR API Maturity](https://nhsconnect.github.io/fhir-policy/publication.html) ‘Publication Requirements’ section of the [NHS FHIR Policy](https://nhsconnect.github.io/fhir-policy/index.html).

