---
title: "A YANG Network Data Model of Network Inventory Software Extensions"
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

The base Network Inventory YANG model defines the physical network
      elements (NEs) and hardware components of NEs. This document extends the
      base Network Inventory model for non-physical NEs (e.g., controllers,
      virtual routers, virtual firewalls) and software components (e.g.,
      platform operating system (OS), software-patch).

--- middle

# Introduction

The Network Inventory consists of the physical and non-physical
      network elements (NEs), hardware components, firmware components, and
      software components on the a managed network domain. The non-physical
      network elements (NEs) are network devices that support network
      protocols and functions, e.g., routers, firewalls, and controllers,
      which can reside in any network or compute devices, such as servers in
      Data Center (DC), server-based virtual machines (VMs), or server-based
      containers.

 {{!I-D.ietf-ivy-network-inventory-yang}}  defines the base
      Network Inventory YANG model for physical network element (NE) and
      hardware components of NEs. Examples of hardware components could be
      rack, shelf, slot, board and physical port.

The management of non-physical NE and software components information
      is similar to the management of physical NE and hardware information.
      For example, inventory data, including product names, serial numbers,
      etc. are also applicable. This document defines a network inventory
      software extension YANG model. In addition to inheriting the common
      inventory attributes of the base network inventory model, this document
      also adds some software-specific attributes of non-physical NEs (such as
      controllers, virtual routers, and virtual firewalls) and software
      components (such as operating system, software patches, BIOS, and boot
      loader).

The Network Inventory software extension model is classified as a
      network model (Section 4 of  {{?RFC8309}}).

The YANG data model in this document conforms to the Network
Management Datastore Architecture (NMDA) defined in {{!RFC8342}}.

## Editorial Note (To be removed by RFC Editor)

Note to the RFC Editor: This section is to be removed prior to publication.

This document contains placeholder values that need to be replaced with finalized values at the time of publication. This note summarizes all of the substitutions that are needed.

Please apply the following replacements:

* XXXX --> the assigned RFC number for this I-D
* AAAA --> the assigned RFC number for {{!I-D.ietf-ivy-network-inventory-yang}}

## Terminology and Notations

The following terms are defined in {{!RFC7950}} and are not
redefined here:

*  client
*  server
*  augment
*  data model
*  data node
   The following terms are defined in {{!RFC6241}} and are not redefined
   here:
*  configuration data
*  state data
   The tree diagram used in this document follows the notation defined
   in {{?RFC8340}}..

 Also, this document uses terms defined in {{!I-D.ietf-ivy-network-inventory-yang}}.

# Requirements Language

  The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
        "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
        "OPTIONAL" in this document are to be interpreted as described in BCP 14
        {{?RFC2119}} {{?RFC8174}} when, and only when, they appear in all capitals, as shown here.

# Relationship to Other YANG Data Models

The base network inventory model supports the software versions of
      NEs and software versions of hardware components. This document adds
      more software component identifiers (e.g. platformos, software patch)
      and more NE types (e.g. software NE, virtual NE) to provide enhanced
      software information on the NE to facilitate software compatibility
      check.

{{fig-ni-sw-mod-relation}} depicts the relationship between
      the Software Extension model and other models. The Software Extension
      network inventory model enhances the model defined in the base network
      inventory model with more software specific attributes.

~~~~ ascii-art
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
|    e.g.,SW patch     |
|                      |
+----------------------+
~~~~
{: #fig-ni-sw-mod-relation title="Relationship of SW Extension Model to Other Inventory Models"}



# Model Overview

The tree diagram in {{full-tree}} provides an overview of the data model for "ietf-network-inventory-sw-ext"
      module.

~~~~~~~~~~
{::include-fold ./ietf-network-inventory-sw-ext.tree}
~~~~~~~~~~
{: #full-tree title="YANG Tree of Software Extensions" artwork-align="center"}

# Non-physical Network Elements

In the base Network Inventory YANG model, "ne-type" is a YANG
      identity that describes the type of the network element and only the
      "physical-network-element" identity" is defined. This document adds
      non-physical NE identity, such as "ne-software", "ne-virtual", and
      "ne-container".

The base Network Inventory model also defines common inventory
      attributes, including the software version, patch versions, product
      name, and serial number. The data is also applicable to non-physical
      NEs.

The Network Inventory software extension mode defines some new
      software attributes, consisting of software status, installation time,
      and activation time.

# Software components

Software components refer to the softwares installed on the NE, such
      as operating system, software patches, BIOS, and boot loaders.

Similar to the common inventory attributes of NEs, the common
      attributes of software components (such as software version, patch
      versions, product name, and serial number) are also applicable to
      software components. For software and patch versions, the base inventory
      (Section 4 of {{!I-D.ietf-ivy-network-inventory-yang}})
      defines the "leaf" of "software-rev" and the "leaf-list" of
      "software-patch-rev". If more detailed installation and activation
      information is needed, the extension attributes of software components
      can be used.

# YANG Data model for Network Inventory Software Extensions

The "ietf-network-inventory-sw-ext" module uses types defined in {{!RFC6991}},
   {{!I-D.ietf-ivy-network-inventory-yang}}.

~~~~ yang
{::include-fold ./ietf-network-inventory-sw-ext.yang}
~~~~
{: sourcecode-markers="true" sourcecode-name="ietf-network-inventory-sw-ext@2024-10-17.yang"}


# Security Considerations

This section uses the template described in {{Section 3.7 of ?I-D.ietf-netmod-rfc8407bis}}.

The "ietf-network-inventory-sw-ext" YANG module defines a data model that is
designed to be accessed via YANG-based management protocols, such as
NETCONF NETCONF {{?RFC6241}} or RESTCONF {{?RFC8040}}. These YANG-based management
protocols (1) have to use a secure transport layer
(e.g., SSH {{?RFC4252}}, TLS {{?RFC8446}}, and QUIC {{?RFC9000}}) and (2) have
to use mutual authentication.

The Network Configuration Access Control Model (NACM) {{!RFC8341}}
provides the means to restrict access for particular NETCONF or
RESTCONF users to a preconfigured subset of all available NETCONF or
RESTCONF protocol operations and content.

There are a number of data nodes defined in this YANG module that are
writable/creatable/deletable (i.e., "config true", which is the
default).  All writable data nodes are likely to be sensitive or
vulnerable in some network environments.  Write
operations (e.g., edit-config) and delete operations to these data
nodes without proper protection or authentication can have a negative
effect on network operations.  The following subtrees and data nodes
have particular sensitivities/vulnerabilities:
* TBC

Some of the readable data nodes in this YANG module may be considered
sensitive or vulnerable in some network environments.  It is thus
important to control read access (e.g., via get, get-config, or
notification) to these data nodes. Specifically, the following
subtrees and data nodes have particular sensitivities/
vulnerabilities:

* TBC


# IANA Considerations

IANA is requested to register the following URI in the "ns" subregistry within
the "IETF XML Registry" {{!RFC3688}}:

~~~~
URI:  urn:ietf:params:xml:ns:yang:ietf-network-inventory-sw-ext
Registrant Contact:  The IESG.
XML:  N/A; the requested URI is an XML namespace.
~~~~

IANA is requested to register the following YANG module in the "YANG Module
Names" registry {{!RFC6020}} within the "YANG Parameters" registry group:

~~~~
Name:  ietf-network-inventory-sw-ext
Namespace:  urn:ietf:params:xml:ns:yang:ietf-network-inventory-sw-ext
Prefix:  nwis
Maintained by IANA?  N
Reference:  RFC XXXX
~~~~

--- back

# Acknowledgments
{:numbered="false"}

The authors would like to thank Prasenjit Manna,Phil Bedard, Diego R.
      Lopez, Italo Busi, and many others for their helpful comments and
      suggestions.

