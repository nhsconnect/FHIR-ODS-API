---
title: Test Overview
keywords: test
tags: [testing]
sidebar: foundations_sidebar
permalink: test.html
summary: "These pages assists with requirements for testing the API."
---
### 1. Testing Overview
The Test section contains a common baseline for testing FHIR based API's to ensure a satisfactory level of technical conformance has been reached. A FHIR based API contains individual layers that require testing, which when combined will form a complete and detailed test log for the API prior to any formal assurance activities being carried out.

Testing may include the following:

- API conformance based on NHS Digital FHIR policy 
- RESTful conformance
- Security
- Authentication
- Payload(s)
- Spine Integration
- Clinical Safety
- End-to-End Testing

Depending on the API, it may be necessary to carry out the additional non-functional testing:

- Penetration Testing
- Performance
- Volumetrics

### 2. FHIR Servers

Where testing requires the use of a FHIR server, there are several options available.

#### 2.1 Public Servers

There are many freely available public servers that can be used to test with.  For a comprehensive list of servers, navigate to [Publicly Available FHIR Servers for testing](http://wiki.hl7.org/index.php?title=Publicly_Available_FHIR_Servers_for_testing)

#### 2.2 Local/Private Server

There are two well supported FHIR servers that can be downloaded and used for testing within your own environment:

**HAPI-FHIR**

A servlet based RESTful server, which is an Open Source application written in Java. More information can be found at [http://hapifhir.io/](http://hapifhir.io/).

**Furore Vonk**

Vonk is created by Furore and is a user friendly RESTful server. It's free for testing, but does require that you restart the server everyday. It can be ran in Docker or as a .NET executable. More information can be found at [https://fhir.furore.com/](https://fhir.furore.com/)


