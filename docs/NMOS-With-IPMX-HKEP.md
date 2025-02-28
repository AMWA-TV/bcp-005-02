{:.no_toc}
{:toc}

_(c) AMWA 2025, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

![NMOS logo](images/NMOS-logo.png)

## Introduction

This document presents the use of IPMX/HKEP compliant Senders and Receivers in an NMOS environment and complements the IPMX technical recommendations [TR-10-5][] (HDCP Key Exchange Protocol - HKEP). Some of the aspects presented in this document are not specific to IPMX compliant devices and apply to the larger family of SMPTE ST 2110 compliant devices.

## Use of Normative Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119][RFC-2119].

## Definitions

The NMOS terms 'Controller', 'Node', 'Source', 'Flow', 'Sender', 'Receiver' are used as defined in the [NMOS Glossary](https://specs.amwa.tv/nmos/main/docs/Glossary.html).

The term 'HKEP' refers to the IPMX Technical Recommendation [TR-10-5][] HDCP Key Exchange Protocol.

## General Provisions

Nodes capable of transmitting HDCP encrypted streams using the IPMX/HKEP protocol MUST have Source, Flow and Sender resources in the IS-04 Node API.

Nodes capable of receiving HDCP encrypted streams using the IPMX/HKEP protocol MUST have Receiver resources in the IS-04 Node API.

Nodes compliant with this specification MUST implement [IS-04][] v1.3 or higher and [IS-05][] v1.1 or higher.

As HDCP encryption applies only to audio and video streams using an RTP transport, any Receiver whose Flow `format` is neither `urn:x-nmos:format:audio` nor `urn:x-nmos:format:video`, or whose `transport` is neither `urn:x-nmos:transport:rtp` nor one of its subclassifications, and any Sender whose Flow `format` is neither `urn:x-nmos:format:audio` nor `urn:x-nmos:format:video`, or whose `transport` is neither `urn:x-nmos:transport:rtp` nor one of its subclassifications, MUST NOT implement this specification.

## HKEP

A Receiver SHOULD provide a `urn:x-nmos:cap:transport:hkep` capability to indicate its support for Senders that use HDCP encryption and the HKEP protocol. A capability value of `true` indicates support for HDCP encryption and the HKEP protocol, while a value of `false` indicates that they are not supported.

A Receiver MAY support either or both `true` and `false` values.

A Controller MUST verify the compliance of Receivers with an active Sender using HDCP encryption and the HKEP protocol by referring to the Sender's SDP transport file `hkep` attribute or by checking the Sender's associated `urn:x-nmos:hkep` attribute. The presence of the `hkep` attribute in an SDP transport file or a value of `true` for the associated `urn:x-nmos:hkep` Sender attribute indicates that the stream is HDCP-protected. Only Receivers supporting HDCP encryption and the HKEP protocol MAY consume such streams.

A Sender compliant with the [HKEP](#hkep) section of this document MUST provide a `urn:x-nmos:hkep` Sender attribute to indicate that HDCP encryption and the HKEP protocol are used by the Sender. This attribute MUST be `true` if an `hkep` attribute is present in the Sender's SDP transport file, and MUST be `false` if no `hkep` attributes are present. If an SDP transport file is not currently available because the Sender is inactive, this attribute indicates whether or not such an SDP transport file would contain an `hkep` attribute if the Sender were active at that time.

> Note: A Sender not providing the `urn:x-nmos:hkep` attribute is either not supporting HDCP encryption and the HKEP protocol or declares itself as not being compatible with the [HKEP](#hkep) section of this document.

A Sender MAY provide a `urn:x-nmos:cap:transport:hkep` capability to indicate that HDCP encryption and the HKEP protocol are supported. A Sender MAY support either or both `true` and `false` values. A Controller MAY use a Sender's `urn:x-nmos:cap:transport:hkep` capability to verify Receiver compliance with the Sender and, if necessary, constrain the Sender to ensure compliance with the Receivers. A Sender constrained to `false` for this capability MUST NOT be part of an HDCP topology and MUST NOT access, produce, or stream HDCP-protected content. A Sender indicates its support for being constrained for this capability by enumerating the `urn:x-nmos:cap:transport:hkep` capability in its [IS-11][] `constraints/supported` endpoint.

> Note: A Sender indicating both `true` and `false` values in its capabilities describes that it may produce both HDCP and non-HDCP streams according to some internal criteria evaluated at activation time.

> Note: HKEP is only available for RTP based transport protocols, all of which use an SDP transport file.

### SDP Transport File

The `hkep` attribute defined in [TR-10-5][] is not yet registered with IANA. If it were, the definition would indicate "Usage Level: session, media" meaning that a session-level `hkep` attribute represents the default value for a media-level `hkep` attribute if the latter is not specified. The SDP transport file may therefore provide `hkep` information either at the session level and/or the media level.

In the HKEP specification, the use of the expression "hkep session attribute" does not define the usage level of the `hkep` attribute but simply refers to it at its most generic level, which is the session level.

### Activation

A Controller MAY activate and configure a Receiver's HDCP encryption and the HKEP protocol using the SDP transport file from a Sender that includes `hkep` attributes.

### Consistency

If the `urn:x-nmos:cap:transport:hkep` capability only allows the value `true`, then the Sender's associated SDP transport file MUST have an `hkep` attribute.

If the `urn:x-nmos:cap:transport:hkep` capability only allows the value `false`, then the Sender's associated SDP transport file MUST NOT have an `hkep` attribute.

If the `urn:x-nmos:cap:transport:hkep` capability allows both `true` and `false` values, then the Sender's associated SDP transport file MUST have an `hkep` attribute when the stream is HDCP-protected and MUST NOT have an `hkep` attribute when the stream is not HDCP-protected.

### HDCP Content Protection

A Receiver implementing [BCP-008-01][] and supporting HDCP encryption and the HKEP protocol MAY notify that the HDCP content protection system prevents the Receiver from accessing or re-transmitting HDCP content using the `streamStatus` and `streamStatusMessage` properties of the Receiver's associated `NcReceiverMonitor`.

A Sender implementing [BCP-008-02][] and supporting HDCP encryption and the HKEP protocol MAY notify that the HDCP content protection system prevents the Sender from accessing or re-transmitting HDCP content using the `essenceStatus` and `essenceStatusMessage` properties of the Sender's associated `NcSenderMonitor`.

### IS-11

For an Input shared by multiple IS-11 Senders that support being constrained for the `urn:x-nmos:cap:transport:hkep` capability to either `true` or `false`, all such Senders MUST be constrained to `false` in order for that Input to become non-HDCP protected. Consequently, if any of these Senders is not constrained to `false`, then those Senders that are constrained to `false` MUST enter the IS-11 `active_constraints_violation` state, because the essence remains HDCP-protected until all relevant Senders are set to false.



[RFC-2119]: https://tools.ietf.org/html/rfc2119 "Key words for use in RFCs"
[IS-04]: https://specs.amwa.tv/is-04/ "AMWA IS-04 NMOS Discovery and Registration Specification"
[IS-05]: https://specs.amwa.tv/is-05/ "AMWA IS-05 NMOS Device Connection Management Specification"
[IS-11]: https://specs.amwa.tv/is-11/ "AMWA IS-11 NMOS Stream Compatibility Management Specification"
[TR-10-5]: https://vsf.tv/download/technical_recommendations/VSF_TR-10-5_2024-02-23.pdf "HDCP Key Exchange Protocol - HKEP"
[BCP-008-01]: https://specs.amwa.tv/bcp-008-01/ "NMOS Receiver Status"
[BCP-008-02]: https://specs.amwa.tv/bcp-008-02/ "NMOS Sender Status"
