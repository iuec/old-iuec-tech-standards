---
title: Messaging Requirements
category: NHS111 IMT Standards
---

Note that this section makes extensive reference to ITK Specifications. These may be
obtained from the TRUD download service, see https://www.uktcregistration.nss.cfh.nhs.uk.
(Further assistance with TRUD may be obtained from the Data Standards and Products
Helpdesk at <mailto:datastandards@nhs.net>.


The relevant TRUD packages are:

- **NHS 111 Version: 1.0; Status: - RC2 Domain Message Specification under NHS
MESSAGE SPECIFICATIONS** – the 111 messaging payload specifications

- **nhs_itkcore** - the ITK core specifications, including web services transport

- **nhs_itkaccreditation** – details of the ITK accreditation process

### 4.1 MSG.1 Payload specifications
Message payloads MUST be conformant with the 111 message definitions

These are specified in NHS 111 Version: 1.0; Status: - RC2 Domain Message Specification
under NHS MESSAGE SPECIFICATIONS.

### 4.2 MSG.2 ITK Distribution Envelope
Messages MUST wrap the 111 message in an ITK Distribution Envelope.

Usage of the Distribution Envelope is also specified in **nhs_itkcore**.

### 4.3 MSG.3 ITK Messaging Architecture
All 111 messaging implementations MUST be compliant with the ITK Messaging Architecture
specification.

The document “**NPFIT-ELIBR-AREL-DST-0433.01 ITK 2.0 Messaging Architecture v2.0**”
(see nhs_itkcore pack) provides further detail about use of the Distribution Envelope, and other generic aspects of ITK message handling (eg versioning, reliable handling). The
requirements in this document MUST be complied with.

(Note that for the purposes of initial 111 go-live then the messaging security requirements (COR-SEC-01 through COR-SEC-05) may be considered to be adequately covered by the approach based on TLS Mutual Authentication that is described in requirement **MSG.6** below).

### 4.4 MSG.4 ITK Web Services Transport
All 111 messaging implementations MUST be compliant with the ITK Web Services
Transport Specification.

The document “**NPFIT-ELIBR-AREL-DST-0430.02 ITK 2.0 Web Services Transport
Specification v3.0**” (see nhs_itkcore pack) provides further detail about the ITK web
services transport. The requirements in this document MUST be complied with, with the exception of the requirements listed below which MAY be omitted for initial 111 go-live:

- The following requirements may be excepted for initial 111 go-live as they are not
relevant as they relate to messaging patterns that are not currently needed by 111:

    - **WS-PAT-02**: Toolkit Web Services MUST use the Asynchronous Invocation style for Pattern 1 services where this is specified

    - **WS-PAT-03**: Toolkit Web Services MUST use the Synchronous Invocation Pattern for Pattern 2 services

    - **WS-PAT-04**: The SimpleMessageResponse MUST contain simple acknowledgement
 of a Pattern 2 request

- The following requirements are security related, and may be excepted for initial 111 go-
live, as an alternative, simplified, security approach has been defined for 111 (see **MSG.6**
below):

    - **WS-SEC-03**: Toolkit Implementations MUST sign the message timestamp

    - **WS-SEC-07**: Toolkit Implementations MUST be able to authenticate a requestor’s identity

    - **WS-SEC-08**: Toolkit Implementations MUST be able to authorise a service request, based on the Service and the Requestor’s identity

    - **WS-DSC-15**: The timestamp element of the SOAP Header MUST be signed

    - **WS-DSC-05**: Canonicalization method MUST be present in the signature

    - **WS-DSC-06**: PKI certificates MUST be used for message signing

    - **WS-DSC-17**: PKI certificates MUST be from a recognised CA

    - **WS-DSC-18**: Toolkit middleware and applications MUST preinstall all CA root certificates that are listed in the Microsoft Trusted Root Certificate Store.

    - **WS-DSC-19**: Toolkit middleware and applications MUST preinstall the NHS CA Root Certificate

    - **WS-DSC-20**: Toolkit middleware and applications MAY preinstall the Root Certificate from other CAs that they choose to trust

    - **WS-DSC-21**: Toolkit middleware and applications MUST verify the certificate Thumbprint against an approved list

    - **WS-DSC-09**: Detached signatures SHOULD be used if XML Signature is utilised

    - **WS-DSC-10**: KeyInfo element MUST use SecurityTokenReference

    - **WS-DSC-14**: The X509 certificate MUST be included in the BinarySecurityToken element

### 4.5 MSG.5 Full ITK Compliance
All 111 messaging implementations SHOULD gain full “ITK Application” Accreditation, including support for the 111 messaging payload bundle.

In practice this will involve, in addition to the above, implementing the remaining requirements in the “itk_core” pack – see “**NPFIT-ELIBR-AREL-DST-0422.02 ITK 2.0 Specifications Overview v2.0**” for details of the additional ITK requirements modules that are relevant to this.


### 4.6 MSG.6 Security based on TLS Mutual Authentication
The 111 service requires “any-to-any” connections between nodes: call handlers and service providers. Connections are made under the direction of information in the 111 directory of service. The size of the handler and provider population, and the requirement to be able to add handlers and providers with a minimum of disruption, argues against reliance on firewalls to restrict connection access. To use firewalls, would require significant reconfiguration across the estate each time a handler or provider is added.


An alternative is to open a single firewall port on each 111 node and rely on certificates and mutually-authenticated TLS to secure connections. This works by requiring that all 111 nodes have a certificate which is identifiable, and trustworthy as, belonging to a bona fide 111 node. On receipt, a connection will only be accepted if it is secured with a certificate that is trustworthy as being from another 111 node. Sites are configured to use this simply by installing the certificate authority certificates, in their platform. Port 1879 has been agreed as the port that will be used by all parties for 111 messaging.


Such trustworthiness is assured by the “policy” which guards the issuance of a certificate to a 111 site. At the time of initial rollout, there is no such established policy which completely assures the identity of a 111 node – delivery of that policy is dependent on a PKI project with a longer delivery timescale than NHS 111.


As an interim, Spine certificates will be used. Spine certificates are signed by a nationally-recognised Certificate Authority, and protected by a policy which identifies sites for connection to Spine. On their own, Spine certificates do not identify a site as a 111 node. However, to create a Spine certificate the Fully Qualified Domain Name (FQDN) and associated IP address of the site must be provided, and the policy enforces this. Therefore, by controlling access to an aspect of the FQDN, the 111 programme ensures that the certificate issued by Spine, identifies the holder as a 111 node.


This will be done by placing all 111 nodes under the subdomain “oneoneone.nhs.uk” – control of that subdomain provides the additional 111-specific policy around the issuance of Spine certificates, that makes a 111-specific certificate trustworthy as such.


On receipt of a connection request, an NHS 111 endpoint will only accept the connection
if it is secured by a Spine-issued certificate that has a CN containing an FQDN of the form
nodename.oneoneone.nhs.uk. In more detail these checks comprise of:

- Full certificate path validation and revocation check by both client and server (client and server stated in the context of the TLS Protocol but in essence, both parties perform verification and validation of each others presented certificates)

- Check the validity of the dates within the certificates presented

- Check that the certificate being presented is one which has been issued to a 111 node
(i.e. has a CN containing an FQDN of the form nodename.oneoneone.nhs.uk)

- Check the issuer of the presented certificate and that should include checking of
Authority Key ID and Subject Key ID


Note that using the FQDN to provide additional identification for a 111 endpoint is a tactical solution and MAY be subject to changes in future policy regarding DNS or use of the 111 services. For example use of the FQDN check might become redundant at such time as 111 security policy is handled via the processes that protect issuance of certificates against a specific 111 sub CA. To avoid undue impact on deployed systems, vendors MUST allow the FQDN checks to be configured such that they can be “turned off” without the need to deploy changed code.


### 4.7 MSG.7 Direct, synchronous connection
To be able to fulfil the requirements for 111 messaging the following properties of the interaction must exist:

- The interface MUST be synchronous from the perspective of an HTTPS connection. The request and response are communicated over the same HTTPS connection and the user must be conscious of the message response in real time.

- The service caller and provider MUST NOT communicate via an intermediary.

### 4.8 MSG.8 Endpoint Addressing
Messages to service providers (eg Out of Hours, Ambulance) will be addressed to the service provider’s endpoint for receiving 111 messages. This service provider endpoint MUST be as retrieved from the 111 Directory of Service (DoS). The location of the DoS itself MUST be a configurable item.

Messages to the Repeat Caller Database will be addressed to a single well-known endpoint.
The details of this Repeat Caller Database endpoint MUST be a configurable item.
