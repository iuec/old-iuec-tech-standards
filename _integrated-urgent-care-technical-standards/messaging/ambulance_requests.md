---
title: Ambulance Requests
category: Integrated Urgent Care Technical Standards
sub_category: Messaging
---

Integrated Urgent Care services have the ability to directly request an ambulance for a patient where necessary. 

A message specification for Ambulance Requests is defined as part of the NHS 111 ITK Domain Message Specification. NHS 111 services, which are using NHS Pathways as a triage tool, are currently able to electronically  request an ambulance where the NHS Pathways tool advises one is required.

These electonic ambulance requests are defined in the [NHS 111 ITK Domain Message Specification](https://isd.hscic.gov.uk/trud3/user/authenticated/group/0/pack/1/subpack/192/releases), which can be downloaded from the [NHS Digital TRUD3 portal](https://isd.hscic.gov.uk/).



### Ambulance requests from clinical hubs

With the introduction of [Clinical Hubs](../integrated-urgent-care/clinical-hubs), electronic ambulance requests will be expanded for 



### Identifying the correct ambulance service

To identify the appropriate ambulance service to send a request to, the postcode of the patient's current location is mapped to the ambulance service responsible for that area by mapping via the responsible Primary Care Organisation which is currently the Clinical Commissioning Group (CCG).

`Postcode -> CCG -> Ambulance Service`


