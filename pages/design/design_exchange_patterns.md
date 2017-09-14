---
title: Exchange Patterns
keywords: design, build
tags: [design, build]
sidebar: foundations_sidebar
permalink: design_exchange_patterns.html
summary: "The Exchange Patterns introduce the three ways of exchanging FHIR Resources using RESTful API, messaging and documents."
---

{% include custom/search.warnbanner.html %}

## 1. Exchange Paradigms ##

This section has been included to show how CareConnectAPI fits in with traditional messaging and document exchanges. Also how these ways of exchanging FHIR resources relates to Information Sharing Patterns.

The exchange patterns are complimentary, each having it's strengths and weaknesses. The best solution will probably to use a combination of pattern.  

## 2. RESTful API

<div markdown="span" class="alert alert-danger" role="alert"><i class="fa fa-fire"></i>  <b><a href="https://www.hl7.org/fhir/http.html"><b>FHIR RESTful API</b></div>

```
Retrieve data held in a remote system while avoiding direct coupling to remote procedures.
```

<p style="text-align:center;"><img src="images/build/FHIR RESTfulAPI.jpg" alt="View Results Screen" title="View Results Screen" style="width:75%"></p>
<br><br>  

Many web services use messages to form their own domain-specific APIs. These messages incorporate common logical commands like Create, Read (i.e. Get), Update, or Delete (CRUD). Also across the health and social care domain, we have many common entities such as Patient, Practitioner, Organisation, etc. For example, many systems will implement an API that allows you to search for Patients by their NHS Number, maybe called GetPatient, QueryPatient, CreatePatient, etc.

Rather than implementing a system specific API, we could utilise standards defined in the HTTP specification (i.e. GET, POST, etc) and standardise how we return the payload.

So rather than a wide variety of GetOrganization, QueryOrganization we have:

<table width="60%">
    <tr>
        <td>
            <b>GET Request</b>
        </td>
        <td>
            <b>Response (server)</b>
        </td>
    </tr>
    <tr>
        <td>
            URI
        </td>
        <td>
            FHIR Resource <br> E.g. Organization
        </td>
    </tr>
</table>

<table width="60%">
    <tr>
        <td>
            <b>POST/PUT Request</b>
        </td>
        <td>
            <b>Response (server)</b>
        </td>
    </tr>
    <tr>
        <td>
            URI + FHIR Resource <br> Organization
        </td>
        <td>
            FHIR Resource <br> E.g. Organization
        </td>
    </tr>
</table>

This type of interface may also be called as **ResourceAPI** and is useful for real time applications and mobile/web based access.

**Benefits**
- Mobile and web application friendly, can be reused for messaging simplifying interfacing.
- Quick to develop
- Real Time Systems
- Uses modern web technologies
- Can be used with modern security and consent technologies ([OAuth2](https://oauth.net/2/), [OpenID](http://openid.net/what-is-openid/), [JWT](https://jwt.io))

**Concerns**
- Less suitable for large transfers of data between organisations and large systems.

### 2.1. Information Sharing Patterns ###

<table width="80%">
<tr>
<td>
  <a href="http://developer.nhs.uk/library/architecture/integration-patterns/portal/"><img class="alignnone size-full wp-image-9872" src="http://developer.nhs.uk/wp-content/uploads/2015/02/tn-Portal-e1422958326475.jpg" alt="tn_Portal" width="251" height="72" /></a>
</td>
<td></td>
</tr>
<tr>
<td>
<a href="http://developer.nhs.uk/library/architecture/integration-patterns/registry-repository/"><img class="alignnone size-full wp-image-9922" src="http://developer.nhs.uk/wp-content/uploads/2015/02/tn-RegistryRepository-e1422959886826.jpg" alt="tn_RegistryRepository" width="250" height="72" /></a>
</td>
<td>Consider using Messaging to populate the Repository</td>
</tr>
<tr>
<td>
<a href="http://developer.nhs.uk/library/architecture/integration-patterns/shared-repository/"><img class="alignnone size-full wp-image-9912" src="http://developer.nhs.uk/wp-content/uploads/2015/02/tn-Repository-e1422959862898.jpg" alt="tn_Repository" width="250" height="72" /></a>
</td>
<td>Consider using Messaging to populate the Repository</td>
</tr>
<tr>
<td>
 <a href="http://developer.nhs.uk/library/architecture/integration-patterns/store-and-notify/"><img class="alignnone size-full wp-image-9832" src="http://developer.nhs.uk/wp-content/uploads/2015/02/tn-StoreAndNotify-e1422958493685.jpg" alt="tn_StoreAndNotify" width="251" height="72" /></a>
</td>
<td></td>
</tr>
<tr>
<td><a href="http://developer.nhs.uk/library/architecture/integration-patterns/publish-subscribe/"><img class="alignnone size-full wp-image-16992" src="http://developer.nhs.uk/wp-content/uploads/2015/02/tn_PubSub_250.jpg" alt="tn_PubSub_250" width="250" height="72" /></a></td>
<td></td>
</tr>
</table>

### 2.2. NHS FHIR Examples ###

- [CareConnectAPI](explore.html)
- [GP Connect]("/gpconnect/")
  - [Tranche 1-3](https://nhsconnect.github.io/gpconnect/accessrecord_rest.html)
  - [Appointment](https://www.simplifier.net/GPConnect/gpconnect-appointment-1)
  - [Order (Task)](https://data.developer.nhs.uk/fhir/candidaterelease-170816-tasks/Profile.TaskManagement/gpconnect-task-order-1.html)
  - [Slot (Appointment)](https://www.simplifier.net/GPConnect/gpconnect-slot-1)
- [Vistors and Migrants](https://nhsconnect.github.io/visitor-and-migrants/index.html)
- [NHS e-Referral Service](https://nhsconnect.github.io/NHS-FHIR-eRS-Integration/Generated/)
- [NHS National Record Locator Service](https://data.developer.nhs.uk/fhir/nrls-v1-draft-a/Chapter.1.About/index.html)

