---
coding: utf-8

title: IETF Network Slice Topology YANG Data Model

abbrev: Network Slice Topology Data Model
docname: draft-liu-teas-transport-network-slice-yang-06
workgroup: TEAS Working Group
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

author:
  -
    name: Xufeng Liu
    org: IBM Corporation
    email: xufeng.liu.ietf@gmail.com
  -
    name: Jeff Tantsura
    org: Microsoft
    email: jefftant.ietf@gmail.com
  -
    name: Igor Bryskin
    org: Individual
    email: i_bryskin@yahoo.com
  -
    name: Luis M. Contreras
    org: Telefonica
    email: luismiguel.contrerasmurillo@telefonica.com
  -
    name: Qin Wu
    org: Huawei
    email: bill.wu@huawei.com
  -
    name: Sergio Belotti
    org: Nokia
    email: Sergio.belotti@nokia.com
  -
    name: Reza Rokui
    org: Ciena
    email: rrokui@ciena.com
  -
    name: Aihua Guo
    org: Futurewei
    email: aihuaguo.ietf@gmail.com
  -
    name: Italo Busi
    org: Huawei
    email: italo.busi@huawei.com

--- abstract

   An IETF network slice may use an abstract topology to describe intended 
   underlay for connectivities between slice endpoints. Abstract topologies
   help the customer to request network slices with shared resources
   amongst connections, and connections can be activated within the slice
   as needed.

   This document describes a YANG data model for managing and
   controlling abstract topologies for IETF network slices defined in
   {{?I-D.ietf-teas-ietf-network-slices}}.

--- middle

# Introduction

   This document defines a YANG {{!RFC7950}} data model for representing,
   managing, and controlling IETF network slices as abstract
   network topologies, where the network slices are defined
   in {{?I-D.ietf-teas-ietf-network-slices}}.

   The defined data model is an interface between customers and
   providers for configurations and state retrievals, so as to support
   network slicing as a service.  Through this model, a customer can
   learn the slicing capabilities and the available resources of the
   provider.  A customer can request or negotiate with a network slicing
   provider to create an instance.  The customer can incrementally
   update its requirements on individual topology elements in the slice
   instance, e.g., adding or removing a node or link, updating desired
   bandwidth of a link, and retrieve the operational states of these elements.
   With the help of other mechanisms and data models defined in IETF,
   the telemetry information can be published to the customer.
   
   The YANG model defines technology-agnostic constructs common to network slicing
   at network layers of different technologies, e.g. IP/MPLS(-TP), OTN and WDM.
   Therefore, this model may be used as a common base model on which other
   network slicing models, such as {{?I-D.ietf-ccamp-yang-otn-slicing}}, may
   augments with technology-specific constructs.

   As described in Section 3 of
   {{?I-D.contreras-teas-slice-controller-models}}, the data model defined
   in this document complements the data model defined in
   {{?I-D.ietf-teas-ietf-network-slice-nbi-yang}}.  In addition to the
   provider's view, the data model defined in this document models the
   Type 2 service defined in {{!RFC8453}}.

   The YANG data model in this document conforms to the Network
   Management Datastore Architecture (NMDA) {{!RFC8342}}.

## Terminology

   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
   "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
   "OPTIONAL" in this document are to be interpreted as described in
   BCP 14 {{!RFC2119}} {{!RFC8174}} when, and only when, they appear in all
   capitals, as shown here.

   The terminology for describing YANG data models is found in
   {{!RFC7950}}.

## Tree Diagram

   Tree diagrams used in this document follow the notation defined in
   {{!RFC8340}}.
   
## Prefixes in Data Node Names

   In this document, names of data nodes and other data model objects
   are prefixed using the standard prefix associated with the
   corresponding YANG imported modules, as shown in {{tab-prefixes}}.

   | Prefix   | YANG Module                  | Reference         |
   |----------|------------------------------|-------------------|
   | yang     | ietf-yang-types              | {{!RFC6991}}      |
   | inet     | ietf-inet-types              | {{!RFC6991}}      |
   | nt       | ietf-network-topology        | {{!RFC8345}}      |
   | nw       | ietf-network-topology        | {{!RFC8345}}      |
   | tet      | ietf-te-topology             | {{!RFC8795}}      |
   | te-types | ietf-te-types                | {{!RFC8776}}      |
   | ns-topo  | ietf-ns-topo                 | RFCXXXX           |
{: #tab-prefixes title="Prefixes and Corresponding YANG Modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please remove this note.

# Modeling Considerations

   An IETF network slice topology is modeled as network topology defined in
   {{!RFC8345}}, with augmentations.  A new network type "network-slice" is
   defined in this document.  When a network topology data instance
   contains the network-slice network type, it represents an instance of
   an IETF network slice topology.

## Relationships to Related Topology Models

   There are several related YANG data models that have been defined in
   IETF.  Some of these are:

   Network Topology Model:
      Defined in {{!RFC8345}}.

   Network Slicing Model:
      Defined in {{?I-D.ietf-teas-ietf-network-slice-nbi-yang}}.

   OTN Slicing:
      Defined in {{?I-D.ietf-ccamp-yang-otn-slicing}}.

   {{fig-model-relationship}} shows the relationships among these models.  The box of
   dotted lines denotes the model defined in this document.
  
~~~~
     +----------+                 +----------+
     | Network  |                 | Network  |
     | Slice    |                 | Topology +
     | NBI YANG >------+          | Model    |
     | Model    |      |          | RFC 8345 |
     +----+-----+      |          +-----+----+
          |            |                |
          |augments    |references      |augments
          |            |                |
     +----^-----+      |          ......^.....
     | OTN      |      +----------: Network  :
     | Slicing  | augments        : Slice    :
     | Model    >-----------------: Topology :
     |          |                 : Model    :
     +----------+                 ''''''''''''
~~~~
{: #fig-model-relationship title="Model Relationships"}
  
## ACTN for Network Slicing

   Since ACTN topology data models are based on the network topology
   model defined in {{!RFC8345}}, the augmentations defined in this
   document are effective augmentations to the ACTN topology data
   models, resulting in making the ACTN framework {{!RFC8453}} and data
   models {{?I-D.ietf-teas-actn-yang}} capable of slicing networks with the
   required network characteristics.

# Model Applicability

   There are many technologies to achieve network slicing.  The data
   model defined in this document can be used to configure resource-based
   network slices, where the resources of a network slice is represented
   in the form of an abstract network topology, which can then be mapped
   to a network resource partition (NRP) according to the scenarios defined
   in {{?I-D.ietf-teas-ietf-network-slices}}.
   
   Network slices may be abstracted differently depending on the requirement
   contained in the configuration provided by the slice customer. A customer
   may request a network slice to provide just connectivity between specified
   endpoints, in which case the network slice can be represented as a set of
   endpoint-to-endpoint links, with each link formed by an end-to-end tunnel
   across the underlying transport networks. The resources associated with
   each link of the slice is reserved and commissioned in the underlying
   physical network upon the completion of configuring the network slice and
   all the links are active.
   
   Alternatively a network slice can also be represented as an abstract topology
   when the customer requests the slice to share resources between multiple
   endpoints and to use the resources on demand. The abstract topology may
   consist of virtual nodes and virtual links, and their associated resources
   are reserved but not commissioned across the underlying transport networks.
   The customer can later commission resources within the slice dynamically 
   using the NBI provided by the service provider. 
   
   According to {{?I-D.ietf-teas-ietf-network-slices}}, the IETF Network Slice
   service customer might ask for some level of control of, e.g., to customize
   the service paths in a network slice. The abstract topology defined in this
   draft could serve to enable this capability and optimize the resource utilization
   for network slice connections activated on top of the abstract topology.
 
   In the example shown in {{fig-ns-topo-example}}, two network resource partitions
   are created by the provider to support the two network slice topology requests
   from the customers.  In realizing the network resource partitions, node virtualization
   is used to separate and allocate resources in physical devices.  Two virtual
   routers VR1 and VR2 are created over physical router R1, and two virtual
   routers VR3 and VR4 are created over physical router R2, respectively.  Each of the
   virtual routers,as a partition of the physical router, takes a portion 
   of the resources such as ports and memory in the physical router.  
   Depending on the requirements and the implementations, they may share 
   certain resources such as processors, ASICs, and switch fabric.
   
   The network slice topology intent requested by the customers is then
   mapped to a corresponding network resource partition. The provider also reports the
   operational state of the topology, which shows the resources that are allocated.
   Customers can process the requested topology and integrate it with their own topology.

   As an example, Appendix B. shows the JSON encoded data instances of
   the customer topology intent for Network Slice Blue.

~~~~
       Customer Topology (Merged)          Customer Topology (Merged)
       Network Slice Blue                  Network Slice Red
                            +---+         +---+      +---+
                       -----|R3 |---   ---|R2 |------|R3 |
                      /     +---+         +---+      +---+                  
      +---+      +---+        ^             ^          ^  \     +---+
   ---|R1 |------|R2 |        |             |          |   -----|R4 |---
      +---+      +---+        |             |          |        +---+
        ^          ^          v             v          v          ^
        |          |        +---+         +---+      +---+        |
        |          |   -----|VR5|---   ---|VR2|------|VR4|        |
        v          v  /     +---+         +---+      +---+        v         
      +---+      +---+                                    \     +---+
   ---|VR1|------|VR3|                                     -----|VR6|---
      +---+      +---+                                          +---+
       Customer Topology (Intended)        Customer Topology (Intended)
       Network Slice Blue                  Network Slice Red

                                 Customers
   ---------------------------------------------------------------------
                                 Provider

        Customized Topology (Network Resouce Partition)
        Provider Network with Virtual Devices

        Network Slice Blue: VR1, VR3, VR5         +---+             
                                        ----------|VR5|------
                                       /          +---+
                    +---+         +---+
              ------|VR1|---------|VR3|                              
                    +---+         +---+
              ------|VR2|---------|VR4|
                    +---+         +---+
                                       \          +---+
                                        ----------|VR6|------
        Network Slice Red: VR2, VR4, VR6          +---+

                                 Virtual Devices 
   ---------------------------------------------------------------------
                                 Physical Devices

        Native Topology
        Provider Network with Physical Devices
                                                  +---+
                                        ----------|R3 |------
                                       /          +---+
                    +---+         +---+
              ======|R1 |=========|R2 |                             
                    +---+         +---+
                                       \          +---+
                                        ----------|R4 |------
                                                  +---+
~~~~
{: #fig-ns-topo-example title="Network Slicing Topologies for Virtualization"} 

# YANG Model Overview
   The following constructs and attributes are defined within the YANG model:

   - Network topology, which represent set of shared, reserved resources organized as a virtual 
      topology between all of the endpoints. A customer could use such network topology
      to define detailed connectivity path traversing the topology, and allow sharing of 
      resources between its multiple endpoint pairs.

   - Service-level objectives (SLOs) associated with different objects, including node, link, 
     termination point of the topology.
   
# Model Tree Structure

~~~~
{::include ./ietf-ns-topo.tree}
~~~~
{: #fig-ietf-ns-topo-tree title="Tree diagram for network slice topology"}
   
# YANG Modules

~~~~
   <CODE BEGINS> file "ietf-ns-topo@2023-03-11.yang"
{::include ./ietf-ns-topo.yang}
   <CODE ENDS>
~~~~
{: #fig-ietf-ns-topo-yang title="YANG model for network slice topology"}   
  
# Manageability Considerations

   To ensure the security and controllability of physical resource
   isolation, slice-based independent operation and management are
   required to achieve management isolation.
   Each network slice typically requires dedicated accounts,
   permissions, and resources for independent access and O&M. This
   mechanism is to guarantee the information isolation among slice
   tenants and to avoid resource conflicts. The access to slice
   management functions will only be permitted after successful security
   checks.

# Security Considerations

   The YANG module specified in this document defines a schema for data
   that is designed to be accessed via network management protocols such
   as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer
   is the secure transport layer, and the mandatory-to-implement secure
   transport is Secure Shell (SSH) {{!RFC6242}}.  The lowest RESTCONF layer
   is HTTPS, and the mandatory-to-implement secure transport is TLS
   {{!RFC8446}}.

   The NETCONF access control model {{!RFC8341}} provides the means to
   restrict access for particular NETCONF or RESTCONF users to a
   preconfigured subset of all available NETCONF or RESTCONF protocol
   operations and content.

   There are a number of data nodes defined in this YANG module that are
   writable/creatable/deletable (i.e., config true, which is the
   default).  These data nodes may be considered sensitive or vulnerable
   in some network environments.  Write operations (e.g., edit-config)
   to these data nodes without proper protection can have a negative
   effect on network operations.  Considerations in Section 8 of
   {{!RFC8795}} are also applicable to their subtrees in the module defined
   in this document.

   Some of the readable data nodes in this YANG module may be considered
   sensitive or vulnerable in some network environments.  It is thus
   important to control read access (e.g., via get, get-config, or
   notification) to these data nodes.  Considerations in Section 8 of
   {{!RFC8795}} are also applicable to their subtrees in the module defined
   in this document.

# IANA Considerations

   It is proposed to IANA to assign new URIs from the "IETF XML
   Registry" {{!RFC3688}} as follows:

~~~~
   URI: urn:ietf:params:xml:ns:yang:ietf-ns-topo
   Registrant Contact: The IESG
   XML: N/A; the requested URI is an XML namespace.
~~~~

   This document registers a YANG module in the YANG Module Names
   registry {{!RFC6020}}.

~~~~
   name: ietf-ns-topo
   namespace: urn:ietf:params:xml:ns:yang:ietf-ns-topo
   prefix: ns-topo
   reference: RFC XXXX
~~~~

--- back

# Acknowledgments

   The TEAS Network Slicing Design Team (NSDT) members included Aijun
   Wang, Dong Jie, Eric Gray, Jari Arkko, Jeff Tantsura, John E Drake,
   Luis M.  Contreras, Rakesh Gandhi, Ran Chen, Reza Rokui, Ricard
   Vilalta, Ron Bonica, Sergio Belotti, Tomonobu Niwa, Xuesong Geng, and
   Xufeng Liu.

# Data Tree for the Example in Section 3.1

## Native Topology
   This section contains an example of an instance data tree in the JSON
   encoding {{!RFC7951}}.  The example instantiates "ietf-network" for the
   topology of Network Slice Blue depicted in {{fig-ns-topo-example}}.
   
~~~~
{::include json-examples/ietf-ns-topo-blue.txt}
~~~~
 
{: numbered="false"}
