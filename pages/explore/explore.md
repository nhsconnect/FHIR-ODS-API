---
title: Resources Overview
keywords: getcarerecord, structured, rest, resource
tags: [rest,fhir,api]
sidebar: foundations_sidebar
permalink: explore.html
summary: "Overview of the Resources section"
---

{% include custom/search.warnbanner.html %}

{% include custom/api_overview.svg %}

## 1. Resource API Structure ##

The FHIR ODS Lookup profile API described in the Explore section of this implementation guide have been structured consistently in the following way:
- `0.` References
- `1.` Read
- `2.` Search Parameters
- `3.` Example

### 1.1 Resource API Structure Details ###

<table style="min-width:100%;width:100%">
<tr id="clinical">
<th style="width:20%;">General</th>
<th style="width:80%;">Description </th>
</tr>
<tr>
<td>0. References</td>
<td>Links to other parts of the implementation guide which might help with context and understanding the API's described</td>
</tr>
<tr>
<td>1. Read</td>
<td>A description of how to get the API</td>
</tr>
<tr>
<td>2. Search Parameters</td>
<td>List of search parameters for the profile being described, including any tips for searching. This section shows examples of how to search using the provided search parameters</td>
</tr>
<tr>
<td>3. Example</td>
<td>Description of of the Request & Response headers, example of how to search on a server and the expected response body as an example</td>
</tr>
</table>

## 2. Resource API's ##
This section looks at the ODS Lookup profile API covered within this implementation guide.


<table style="min-width:100%;width:100%">

<tr>
<th>Entities</th>
</tr>
<tr>
<td><a href="api_entity_organisation.html">Organisation</a></td>
</tr>
</table>


<table style="min-width:100%;width:100%">
<th>Terminology</th>

<tr>
<td><a href="api_foundation_valueset.html">ValueSet</a></td>
</tr>
</table>