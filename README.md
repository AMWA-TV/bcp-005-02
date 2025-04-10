# \[Work In Progress\] AMWA BCP-005-02 NMOS Support for IPMX/HKEP 

[![Lint Status](https://github.com/AMWA-TV/bcp-005-02/workflows/Lint/badge.svg)](https://github.com/AMWA-TV/bcp-005-02/actions?query=workflow%3ALint)
[![Render Status](https://github.com/AMWA-TV/bcp-005-02/workflows/Render/badge.svg)](https://github.com/AMWA-TV/bcp-005-02/actions?query=workflow%3ARender)

This repository holds the source for this Specification, part of the family of [Networked Media Open Specifications](https://specs.amwa.tv/nmos) from the [Advanced Media Workflow Association](https://amwa.tv)

<!-- INTRO-START -->

### What does it do?

- Establishes a standardized mechanism for NMOS Senders and Receivers to declare support for HDCP over IP streaming using IPMX/HKEP (VSF TR-10-5).

### Why does it matter?

- An NMOS Controller can verify that any Receiver is compliant with a Sender producing HDCP-encrypted streams, as only IPMX/HKEP-compliant Receivers can process such content.
- A standardized framework enables Controllers to systematically verify and/or enforce compliance.

### How does it work?

- Documents how IPMX/HKEP capabilities are announced by both Senders and Receivers.
- Explains how to constrain a Sender’s IPMX/HKEP capability.
- Outlines the requirements for Senders, Receivers, and Controllers regarding the IPMX/HKEP feature.

<!-- INTRO-END -->

## Getting started

There is more information about the NMOS Specifications and their GitHub repos at <https://specs.amwa.tv/nmos>.
