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

   An IETF network slice may use a customized topology to describe intended
   resource reservation for connectivities between slice endpoints. Customized
   topologies enable customers to request shared resources among connections activated
   on demand and to customize the service paths in a network slice with 
   an extensive level of control.
   
   This document describes a YANG data model for managing and
   controlling customized topologies for IETF network slices defined in
   RFC YYYY. 
   
   [RFC EDITOR NOTE: Please replace RFC YYYY with the RFC number of
   draft-ietf-teas-ietf-network-slices once it has been published.

--- middle

# Introduction

   {{?I-D.ietf-teas-ietf-network-slices}} describes that an IETF network
   slice service customer might ask for some level of control, e.g., 
   to customize the service paths while requesting network slice services.
   These customized controls may well be expressed by customer-defined 
   topologies, i.e. customized topologies.
   
   Furthermore, by using customized topologies, customers can request advanced
   resource reservation for all potential network slices that can be activated 
   on demand in the future. This capability provides customers with full control
   over when and how resources are allocated by the slices. The resources can be
   shared by network slices created at different times as well as by connections
   between different edge pairs within the same network slice. This could significantly
   reduce overall bandwidth requirements and provide great economic advantages
   to the customer.

   A typical example of network slicing with resource reservation using 
   customized topologies is in recursive network slicing for
   network wholesale, where [add example]
   
   This document defines a YANG {{!RFC7950}} data model for representing,
   managing, and controlling IETF network slices over customized
   network topologies, where the network slices are defined
   in {{?I-D.ietf-teas-ietf-network-slices}}. The YANG model augments
   the data model defined in {{!RFC8345}} to enable a customer to 
   express desired service-level objectives (SLOs) and service-level expectations (SLEs)
   over various constructs within the customized topology.

   The defined data model is an interface between customers and
   providers for configurations and state retrievals, so as to support
   network slicing as a service.  Through this model, a customer can request or 
   negotiate with a network slicing provider to create an instance.  
   The customer can incrementally update its requirements on individual 
   topology elements in the slice instance, e.g., adding or removing a node or 
   link, updating desired bandwidth of a link, and retrieve the operational states
   of these elements. With the help of other mechanisms and data models defined 
   in IETF, the telemetry information can be published to the customer, too.
   
   The YANG model defines constructs that are technology-agnostic to network slicing
   built on network layers with different technologies, e.g. IP/MPLS, MPLS-TP, 
   OTN and WDM optical. Therefore, this model can serve as a common and base model
   on which technology-specific models for network slicing, such as 
   {{?I-D.ietf-ccamp-yang-otn-slicing}}, may be built.

   As described in Section 3 of
   {{?I-D.contreras-teas-slice-controller-models}}, the data model defined
   in this document is optional for network slicing, and complements the data model
   defined in {{?I-D.ietf-teas-ietf-network-slice-nbi-yang}}. In addition to the
   provider's view, the data model defined in this document models the
   Type 2 service defined in {{?RFC8453}}.

   The YANG data model in this document conforms to the Network
   Management Datastore Architecture (NMDA) {{!RFC8342}}.

## Terminologies and Notations

   The following terminologies for describing network slices are defined in 
   {{?I-D.ietf-teas-ietf-network-slices}} and are not redefined herein.
   - Network Slice
   - Network Slice Customer
   - Network Slice Service Provider
   - Network Slice Controller
   - Network Resource Partition (NRP)
   
   The following terms are defined and used in this document.
   
   - Customized Topology:
       A topology defined by the customer and served as an input to the network slice 
       service provider, i.e. to the Network Slice Controller (NSC).
   - Abstract Topology:
       A topology exposed to the customer by the network slice service provider prior to
       the creation of network slices. The provider uses an abstract topology expose
       useful information, such as available resources to the customer.
   - NRP Topology:
       A topology internal to the NSC to facilitate the mapping of network slices to
       underlying network resources.

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

   An IETF network slice topology is a customized topology 
   modeled as network topology defined in {{!RFC8345}}, with augmentations. 
   A new network type "network-slice" is defined in this document.  
   When a network topology data instance contains the network-slice 
   network type, it represents an instance of an IETF network slice 
   topology.

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

   Since ACTN topology data models defined in {{!RFC8795}} are based on the network topology
   model defined in {{!RFC8345}}, the augmentations defined in this
   document are effective augmentations to the ACTN topology data
   models, resulting in making the ACTN framework {{?RFC8453}} and data
   models {{?I-D.ietf-teas-actn-yang}} capable of slicing networks with the
   required network characteristics.
   
   The ACTN topology {{!RFC8795}} and tunnel {{?I-D.ietf-teas-yang-te}} data models
   can also be considered as viable models for network slice realization. In this
   case the ACTN data models are used at the MDSC-to-PNC interface (MPI), 
   at the southbound of the NSC, according to {{!RFC8453}} and 
   {{?I-D.ietf-teas-applicability-actn-slicing}}.

# Model Applicability

   There are many technologies to achieve network slicing.  The data
   model defined in this document can be used to configure resource-based
   network slices, where the resources for network slices are reserved
   and represented in the form of a customized topology, which can then be mapped
   to a network resource partition (NRP) according to the scenarios defined
   in {{?I-D.ietf-teas-ietf-network-slices}}.
   
   Network slices may be abstracted differently depending on the requirement of 
   the network slice customer. A customer may request a network slice with
   direct connectivity between pairs of service demarcation points (SDPs).
   Each connection within the network slice may be further supported by an 
   end-to-end tunnel that traverse a specific path in a given customized topology
   supplied by the customer. The resources associated with a link is immediately
   commissioned by the realization at the time of network slice configuration.
   
   Alternatively, a customer may initially request resources to be reserved for
   potential network slices through a customized topology, and use the resources
   to build network slices in the future. The customized topology represents resources that
   are reserved but not commissioned at the time of request. By that way, the customer can share 
   resources between multiple endpoints and use the resources on demand. 
      
   In the example shown in {{fig-ns-topo-example}}, two customized topology intents named as
   Network Slice Blue and Network Slice Red, are created
   by separate customers and delivered to the network slice service provider.
   The provider maps the two intents internally to corresponding network resource partitions (NRPs)
   internally. In realizing the network resource partitions, node virtualization
   is used to separate and allocate resources in physical devices.  Two virtual
   routers VR1 and VR2 are created over physical router R1, and two virtual
   routers VR3 and VR4 are created over physical router R2, respectively.  Each of the
   virtual routers,as a partition of the physical router, takes a portion 
   of the resources such as ports and memory in the physical router.  
   Depending on the requirements and the implementations, they may share 
   certain resources such as processors, ASICs, and switch fabric.
   
   The provider reports the operational state of the topology, which represents the allocated
   resources agreed upon through negotiations between the customer and the provider.
   Customers can subquently process the requested topology and integrate it into their own topology.
   Note that this relationship between the customer and provider can be recursive, e.g.,
   a customer for network slices can be a provider of its own to offer
   network slice services with customized topologies to its own customer further above
   the hierarchy.

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
