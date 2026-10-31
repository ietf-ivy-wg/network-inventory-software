---
title: "A YANG Network Data Model of Network Inventory Software"
abbrev: "Network Inventory Software"
category: std

docname: draft-ietf-ivy-network-inventory-software-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: ""
workgroup: "Network Inventory YANG"
keyword:

  - Automation
  - Network Inventory
  - Network Operation

author:
  - name: Bo Wu
    org: Huawei
    email: lana.wubo@huawei.com
  - name: Cheng Zhou
    org: China Mobile
    email: zhouchengyjy@chinamobile.com
  - name: Qin Wu
    org: Huawei
    email: bill.wu@huawei.com
  - name: Mohamed Boucadair
    org: Orange
    email: mohamed.boucadair@orange.com

contributor:
-
    name: Yao Zhao
    org: Huawei
    email: zhaoyao.zhaoyao@huawei.com

normative:

informative:

--- abstract

This document extends the base Network Inventory YANG model to support
software-based network elements (NEs) and software components. The
extension introduces software lifecycle status that distinguishes running
software from software that is installed but not running, together
with installation timestamps for non-running software, and both
installation and activation timestamps for running software.

--- middle

# Introduction

The Network Inventory consists of the physical and software-based
      network elements (NEs), hardware components, firmware components, and
      software components on a managed network domain. The software-based
      network elements (NEs) are network devices that support network
      protocols and functions, e.g., routers, firewalls, and controllers,
      which can reside in any network or compute environment.

 {{!I-D.ietf-ivy-network-inventory-yang}}  defines the base
      Network Inventory YANG model for physical network element (NE) and
      hardware components of NEs. Examples of hardware components could be
      rack, shelf, slot, board and physical port.

The management of software-based NE and software components information
      is similar to the management of physical NE and hardware information.
      For example, inventory data, including product names, serial numbers,
      etc. are also applicable. This document defines a network inventory
      software YANG model. In addition to inheriting the common
      inventory attributes of the base network inventory model, this document
      also adds some software-specific attributes of software-based NEs (such as
      controllers, virtual routers, and virtual firewalls) and software
      components (such as operating system, software modules, BIOS, and boot
      loader).

The Network Inventory software model is classified as a network model ({{Section 4 of  ?RFC8309}}).

## Editorial Note (To be removed by RFC Editor)

Note to the RFC Editor: This section is to be removed prior to publication.

This document contains placeholder values that need to be replaced with finalized values at the time of publication. This note summarizes all of the substitutions that are needed.

Please apply the following replacements:

* XXXX --> the assigned RFC number for this I-D
* AAAA --> the assigned RFC number for {{!I-D.ietf-ivy-network-inventory-yang}}

## Terminology and Notations

{::boilerplate bcp14}

The following terms are defined in {{!RFC7950}} and are not
redefined here:

*  client
*  server
*  augment
*  data model
*  data node

The tree diagram used in this document follows the notation defined
   in {{?RFC8340}}.

Also, this document uses terms defined in {{!I-D.ietf-ivy-network-inventory-yang}}, specifically:

*  Network Inventory: See {{Section 2.2 of I-D.ietf-ivy-network-inventory-yang}}.
*  Network Element: See {{Section 2.2 of I-D.ietf-ivy-network-inventory-yang}}.
*  Component: See {{Section 2.2 of I-D.ietf-ivy-network-inventory-yang}}.
*  Software Components: See {{Section 3.3.2 of I-D.ietf-ivy-network-inventory-yang}}.

# YANG Data Model for Network Inventory Software Overview

## Relationship to Other YANG Data Models

The base network inventory model supports the software versions of
NEs and software versions of hardware components. This document adds
more software component identifiers (e.g., platform OS), software NE type,
and software lifecycle status and time stamp information to provide extended
software information on the NE to facilitate software compatibility check.

{{fig-ni-sw-mod-relation}} depicts the relationship between
the network inventory software model and the base network inventory model.
The network inventory software model adds more software specific attributes.

~~~~ aasvg
+----------------------+
|                      |
|Base Network Inventory|
|                      |
+----------^-----------+
           |
           |
           |
           |
+----------^-----------+
|                      |
|  Software Extensions |
|    e.g., SW module   |
|                      |
+----------------------+
~~~~
{: #fig-ni-sw-mod-relation title="Relationship of SW Extension Model to the Base Inventory Model" artwork-align="center"}

## Network Inventory Software Tree Diagram

The tree diagram in {{full-tree}} provides an overview of the data model for "ietf-network-inventory-software"
      module.

The model applies software extension attributes at two levels:

- NE-level "software-rev": Describes the main system software
  revision of the network element, such as the network
  operating system release of the whole device. Only running
  software is reported here, consistent with the base model.
  The augmented nodes	include installation-time and activation-time.

- Component-level "software-rev": Describes the independent software
  or firmware revision of an individual component, such as a line
  card software, a line card firmware, or a pluggable software module.
  Only running software is reported here, consistent with the base model.
  The augmented nodes	include installation-time and activation-time.

In addition, the model introduces:

- "installed-software": Lists at both NE and component levels,
   reporting software that is present on the entity but not currently
   running (installed, or activated but pending reload).
   installed-software carries name, revision, status, installation-time.
   Activation-time is not modeled for installed-software entries;
   activation-time is only available once the software becomes running
   and appears in software-rev.

~~~~~~~~~~
{::include-fold ./ietf-network-inventory-software.tree}
~~~~~~~~~~
{: #full-tree title="YANG Tree of Network Inventory Software" artwork-align="center"}

## Software-based Network Elements

In the base Network Inventory YANG model, "ne-type" is a YANG
identity that describes the type of the network element and only the
"ne-physical" identity" is defined. This document adds
"ne-software-based" identity for software-based NEs.

The base Network Inventory model also defines common inventory
attributes, including the software version, patch versions, product
name, and serial number. The data is also applicable to software-based
NEs.

A software-based NE performs network protocol or forwarding functions
but does not correspond to a distinguishable hardware chassis.
It may be hosted on bare-metal server, in a virtual machine,
container, or cloud compute instance.

## Software Components

Software components refer to the software installed on the NE, such
as operating system, software modules, BIOS, and boot loaders.

Similar to the common inventory attributes of NEs, the common
attributes of software components (such as software revisions,
patch revisions, product name, and serial number) are also applicable to
software components. For software revisions and patch revisions, the base inventory
({{Section 4 of !I-D.ietf-ivy-network-inventory-yang}})
defines the "list" of "software-rev" and the "list" of
"patch". For example, on a router, software components may
include a routing protocol package (e.g., "foo-rt-protocol-suite"),
or a firmware module for a line card (e.g., "foo-lc-fw-21.5.3").

The base "software-rev" list reports software that is currently
   running on the entity.  Software that is present but not running
   (e.g., installed but not activated, or activated but pending a reload
   for a cold patch or cold upgrade) is reported in the
   "installed-software" list defined by this document.  The two lists
   are mutually exclusive: an entry MUST NOT appear in both lists at the
   same time.

# YANG Data Model for Network Inventory Software

The "ietf-network-inventory-software" module uses types defined in {{!RFC9911}} and {{!I-D.ietf-ivy-network-inventory-yang}}.

~~~~ yang
{::include-fold ./ietf-network-inventory-software.yang}
~~~~
{: sourcecode-markers="true" sourcecode-name="ietf-network-inventory-software@2026-10-31.yang"}

# Operational Considerations

   This section complements the operational considerations of the
   base network inventory model {{!I-D.ietf-ivy-network-inventory-yang}}.
   The considerations of {{!I-D.ietf-ivy-network-inventory-yang}}
   also apply to this extension: the read-only perspective of
   inventory data, data discovery by the server from underlying
   elements, aggregation at hierarchical controller interfaces,
   and brownfield migration scenarios.

   This model provides read-only reporting of software status and
   associated timestamps.  It reflects the state of software
   revisions and patches as discovered by the server; it does not
   track software configuration intent or repository state.

   The model is not intended to expose every internal software component
   present on the network element. It only covers operator-relevant, separately
   versionable and independently upgradable software artifacts, such as base OS images,
   line-card FPGA firmware, separable feature modules, and hotfix patches. Embedded static
   libraries and fine-grained internal sub-components that cannot be independently managed
   or upgraded are out of scope.

   The base 'software-rev' list reports running software only.  Presence
   of an entry in 'software-rev' implies that the software is running.
   The 'installed-software' list reports software that is present but
   not running, including software that is installed but not activated,
   and software that has been activated but is pending a reload (cold patch
   or cold upgrade).

   Full software lifecycle management (install, activate, rollback,
   remove operations, etc.) is out of scope of this document and could be
   defined in separate YANG modules.

# Security Considerations

This section is modeled after the template described in {{Section 3.7
of ?RFC9907}}.

The "ietf-network-inventory-software" YANG module augments the base
   network inventory model defined in {{!I-D.ietf-ivy-network-inventory-yang}}.
   All security considerations specified in the base model apply to this
   extension module. This section complements the base model with
   additional security considerations specific to inventory software
   extensions.

The "ietf-network-inventory-software" YANG module defines a data model that is
designed to be accessed via YANG-based management protocols, such as
Network Configuration Protocol (NETCONF)  {{?RFC6241}} and RESTCONF {{?RFC8040}}. These YANG-based management
protocols (1) have to use a secure transport layer (e.g., Secure Shell (SSH) {{?RFC4252}}, TLS {{?RFC9846}},
and QUIC {{?RFC9000}}) and (2) have to use mutual authentication.

The Network Configuration Access Control Model (NACM) {{!RFC8341}}
provides the means to restrict access for particular NETCONF or
RESTCONF users to a preconfigured subset of all available NETCONF or
RESTCONF protocol operations and content.

Some of the readable data nodes in this YANG module may be considered
sensitive or vulnerable in some network environments.  It is thus
important to control read access (e.g., via get, get-config, or
notification) to these data nodes. Specifically, the following subtrees and data nodes have particular sensitivities/vulnerabilities:

- "/nwi:network-elements/nwi:network-element/nwi:software-rev"
- "/nwi:network-elements/nwi:network-element/nwi:software-rev/nwi:patch"
- "/nwi:network-elements/nwi:network-element/nwis:installed-software"
- "/nwi:network-elements/nwi:network-element/nwi:components/nwi:component/nwi:software-rev"
- "/nwi:network-elements/nwi:network-element/nwi:components/nwi:component/nwi:software-rev/nwi:patch"
- "/nwi:network-elements/nwi:network-element/nwi:components/nwi:component/nwis:installed-software"

These extensions add installation and activation timestamps for
running software, plus a separate installed-software list for both
NE-level and component-level. Disclosure of detailed
software versions, patch levels, and lifecycle timestamps can reveal
unpatched vulnerabilities and operational maintenance windows,
enabling targeted attacks.

# IANA Considerations

IANA is requested to register the following URI in the "ns" subregistry within
the "IETF XML Registry" {{!RFC3688}}:

~~~~
URI:  urn:ietf:params:xml:ns:yang:ietf-network-inventory-software
Registrant Contact:  The IESG.
XML:  N/A; the requested URI is an XML namespace.
~~~~

IANA is requested to register the following YANG module in the "YANG Module
Names" registry {{!RFC6020}} within the "YANG Parameters" registry group:

~~~~
Name:  ietf-network-inventory-software
Namespace:  urn:ietf:params:xml:ns:yang:ietf-network-inventory-software
Prefix:  nwis
Maintained by IANA?  N
Reference:  RFC XXXX
~~~~

--- back

# Examples of Software Attributes

This appendix illustrates, by means of four typical scenarios, how to
 populate the software-specific nodes defined in
 "ietf-network-inventory-software".

Scenario 1: Whole-device base software (example-os) running, with a
   hot patch (P3) already running and a hot patch (P4) installed but not
   yet activated. A line-card programmable forwarding image (example-fpga-image)
   is running, with a cold patch (2.1.0.P1) administratively activated and pending reload.
   An independent telemetry software module (telemetry-module) is running, with a newer
   revision (1.6.0) installed but not yet activated.

Scenario 2: Whole-device cold upgrade, where a new base software revision (8.0.0) has been
 installed and is pending reload.

Scenario 3: Software-based virtual router running its base software (vrouter-os)
 with an active hotfix (Hotfix-05), and a newer base revision (24.06) installed but not yet activated.

Scenario 4: Whole-device base software (example-os) running, with an independent hotfix (os-hotfix-P5)
modelled as a separate top-level software-rev entry.

~~~~ json
{::include-fold ./software-example.json}
~~~~

# Acknowledgments
{:numbered="false"}

The authors would like to thank Chongfeng Xie, Prasenjit Manna, Phil Bedard, Diego R.Lopez,
Italo Busi, Adrian Farrel, and many others for their helpful comments and
suggestions. The authors also thank Reshad Rahman for the YANG doctor review.

