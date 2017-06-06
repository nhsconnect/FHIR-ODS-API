---
title: Maturity Model
keywords: development,
tags: [engage,development,fhir]
sidebar: overview_sidebar
permalink: design_maturity_model.html
summary: High-level maturity model for the DoH Cost Recovery programme.
---

[<i class="fa fa-arrow-left" aria-hidden="true"/> Back to Visitors and Migrants - Introduction.](index.html)

{% include important.html content="All development phases outlined below are indicative and subject to on-going review." %}

The maturity roadmap of the development phases of the DoH Cost Recovery Programme are outlined below:

## Phase 1 ##
- Implementation of a data feed from the Home Office (HO) to NHS Digital identifying non-EEA patients that have paid the IHS or are exempt or already eligible for free NHS Services.
- The data provided by the HO results in one of the following statuses being recorded against the identified patients:
	- Paid or exempt from the health surcharge.
	- Likely chargeable for NHS Services
	- Patient is now a British Citizen.
 
## Phase 2 ##
- Functionality in SCRa for authorised Overseas Visitors Managers (OVM) to record pertinent information relating to their assessment of a patient's likely chargeable status. This would enable sharing of information between OVMs across trusts and secondary care units. 
- As well as recording information such as EHIC and S1/S2 details an OVM could indicate one of the following chargeable status categories:
	- A:	Ordinarily resident in the UK (according to the domestic Charging Regulations)
	- B:	Resident in UK, in possession of a valid visa AND (surcharge paid OR exempt)
	- C:	Ordinarily resident in another EEA country AND (EHIC/PRC/S2 OR exempt)
	- D:	Ordinarily resident in another EEA country AND (No EHIC/PRC/S2 OR not exempt)
	- E:	Not ordinarily resident in the UK, not paid, but exempt by personal status or treatment
	- F:	Not ordinarily resident in the UK or EEA, not paid, not exempt by personal status or treatment

- The explicit display of this chargeable status (whether chargeable or not) will only apply for overseas patients identified by the HO or for patients who have been subject to an OVM assessment. 

## Phase 3  ##
- An API interface enabling accredited supplier-systems to request the Spine for the chargeable status indicator for a patient.
- This request will be 'read-only' and will result with the following included in the response:
	- A patient basic chargeable-status indicator
	- A patient category chargeable-status
	- The date of the latest HO or OVM update to chargeable-status
	
- This chargeable status (whether chargeable or not) will only apply for overseas patients identified by the HO or for patients who have been subject to an OVM assessment. 


## Phase 4  ##
- Implementation of additional data-feeds into NHS Digital providing the chargeable-status of additional patients.

## Phase 5  ##
- Modifications to Secondary Care systems to enable use of the chargeable-status indicator made available via the Spine API interface.