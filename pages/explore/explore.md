---
title: Explore Overview
keywords: getcarerecord, structured, rest, resource
tags: [rest,fhir]
sidebar: foundations_sidebar
permalink: explore.html
summary: "Overview of the Explore section"
---

{% include custom/search.warnbanner.html %}

{% include custom/api_overview.svg %}


## 1. Profile Structure ##
The FHIR profiles described in the Explore section of this implementation guide have been structured consistently in the following way:
0. References
1. Read
2. Search Parameters
3. Example

### 1.1 Profile Structure Details ###

<table style="min-width:100%;width:100%">
<tr id="clinical">
<th style="width:20%;">General</th>
<th style="width:80%;">Description </th>
</tr>
<tr>
<td>0. References</td>
<td>Links to other parts of the implementation guide which might help with context and understanding the profiles described</td>
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

## 2. Profiles ##
This section looks at the profiles covered within this implementation guide.




<table style="min-width:100%;width:100%">
<tr id="base">
<th style="width:33%;">Base</th>
<th style="width:33%;">&nbsp;</th>
<th style="width:33%;">&nbsp;</th>
</tr>
<tr id="based">
<th>Individuals</th>
<th>Entities</th>
<th>Workflow</th>
</tr>
<tr>
<td></td>
<td><a href="restfulapis_identification_organisation.html">Organization</a></td>
<td></td>
</tr>
</table>


<table style="min-width:100%;width:100%">
<tr id="conformance">
<th style="width:33%;">Foundation</th>
<th style="width:33%;"></th>
<th style="width:33%;"></th>
</tr>
<tr id="conformanced">
<th>CapabilityStatement</th>
<th>Terminology</th>
<th>&nbsp;</th>
</tr>
<tr>
<td><a href="restfulapis_capabilitystatement_capabilitystatement.html">CapabilityStatement</a></td>
<td><a href="restfulapis_terminology_valueset.html">ValueSet</a> <a href="restfulapis_terminology_codesystem.html">CodeSystem</a></td>
<td>&nbsp;</td>
</tr>
</table>
