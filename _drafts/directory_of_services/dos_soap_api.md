# Using the Directory of Services (DoS) API

## Introduction
### About the web services

The Pathways Web Service supports multiple methods to allow external systems tointerrogate the Pathways Directory of Service (DoS) and allows for the retrieval ofspecific reference data that is relevant for searching the DoS.

The following methods are supported in the Pathways Web Service:

- CheckCapacity
- CheckCapacitySummary
- Dispositions
- ExceptionCapture
- ServiceCare
- ServiceCareException
- ServiceDetailsById

SOAP versions 1.1 and 2.0 are currently supported and all examples within thisdocument are defined as SOAP 2.0 request/responses.

The Pathways web service does not provide any triaging functionality, it is theresponsibility of external systems to triage and collect the relevant informationrequired to make Pathways web service requests.



### About this guide

This guide provides comprehensive documentation on how to use and what toexpect to be returned for each of the methods mentioned above inc error definitionsfor each method.

Note this documentation refers to the v 1.1 of the Pathways web service and theterm “ServiceCare item” is now referred to as “Service”.



### Assumptions

- The reader has an understanding of SOAP and associated technologies
- The reader has an understanding of Pathways terminology




## Methods
### CheckCapacity
This method returns appropriate services based on the specific clinical/locational criteria for a patient. This web service method does not provide any clinical triaging functionality.

The method will only return service ids and the current capacity status of a service. If further service details are required then an additional call to the ServiceCare method, passing in the service id, will provide this extra information.

NOTE:

- The CheckCapacitySummary method provides the same functionality but returns the services full details in one request. CheckCapacitySummary should be used, as CheckCapacity may be removed in a future release.

#### Request Definition

|                          | Type           | Optional | Comments                 |
| ------------------------ | -------------- | -------- | ------------------------ |
| soap:Header              |                |          |                          |
| serviceVersion           | string         | no       | current version is "1.3" |
| soap:Body                |                |          |                          |
| userInfo                 | **_UserInfo_** |          |                          |
| username                 | string         | yes      |                          |
| password                 | string         | yes      |                          |
| c                        | **_Case_**     |          |                          |
| caseRef                  | string         | yes      |                          |
| caseId                   | string         | yes      |                          |
| postcode                 | string         | no       |                          |
| surgery                  | string         | yes      |                          |
| symptomGroup             | int            | no       |                          |
| symptomDiscriminatorList | Array: int     | no       |                          |
| searchDistance           | int            | yes      |                          |
| gender                   | string         | yes      | M / F / I                |



#### Request Definition

CheckCapacityResult - options; type ArrayOfServiceCareDestination

- ServiceCareDestination - optional, unbounded, millable; type ServiceCareDestination
  - id type int
  - capacity type Capacity - type string with restriction - enum ('High', 'Low', 'None')

| Web Service return value | Maps to DoS Capacity Indicator |
| ------------------------ | ------------------------------ |
| High                     | Green                          |
| Low                      | Amber                          |
| None                     | Red                            |



