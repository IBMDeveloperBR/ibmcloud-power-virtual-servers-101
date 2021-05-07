# IBM Power Systems Virtual Servers on IBM Cloud

[![service-card](../images/service-card.png)](https://cloud.ibm.com/catalog/services/power-systems-virtual-server)

## 1. Introduction

### 1.1. Why use IBM Power Systems?

IBM Power Systems are based on proprietary IBM POWER processors used on most of the contemporary super-computers and enterprise-grade compute infrastructure. The ITIC Global Reliability report rates IBM POWER9 Systems the most reliable commercially available compute platform, with a guaranteed reliability of 99.999%. [Check the 2020 ITIC report](https://www.ibm.com/account/reg/us-en/signup?formid=urx-39584). The "five nines uptime" or "99.999% or greater availability" equate to **at least** 5.26 minutes of per server/per annum downtime. IBM POWER9 goes further, accounting only for 1.75 minutes of downtime a year.

Other IBM Power Systems notable reliability achievements include:

- Delivered the highest availability for the 12th year in a row
- Rated first or second in every reliability category
- Received record best in availability
- Rated excellent in configuration & setup

On top of being the most reliable compute platform avaiable, IBM Power Systems servers have an increased performance and a smaller server footprint compared to x86 architectures. In other words, IBM Power Systems are capable of computing more with less cores, packing a denser punch when crunching workloads. [Check some real success cases of IBM Power Systems for Enterprise workloads](https://www.ibm.com/downloads/cas/2P6X2L8G).

### 1.2. Underlying infrastructure environment

IBM Power Systems Virtual Servers are an Infrastructure-as-a-Service offering available at IBM Cloud. Virtual servers built on top of POWER processor systems are co-located physically and connected through low-latency networks to IBM Cloud data centers. Consumers which traditionally relied only on on-premises infrastructure can now tap Power Systems infrastructure on demand in a public cloud environment for boosting a Hybrid Cloud approach, or eliminating completely the capital risks associated with on-premises infrastructure acquisition and maintainance.

At the data centers, the Power Virtual Servers infrastructure is physically separated from the standard x86 machines through a fenced network, which is also directly attached to storage. However, it is possible to connect Power Virtual Servers with other IBM Cloud public or on-premises infrastructure if desired. This architecture is identical to the on-premises Power Systems, and allows Power Virtual Servers to retain the same Enterprise Certifications from its on-premises counterpart.

### 1.3. Available Cloud Regions and Zones

The IBM Power Systems Infrastructure network, is built upon IBM Power's enterprise-grade secure networking hardware and connectivity; it can be bridged to the separate IBM Cloud networks (either Classic Infrastructure network or VPC Infrastructure network). The following geographies, locations, Cloud Regions and Zones have Power Systems VS's available:

| Geography    | Location           | Region   | Zones            | Co-located Classic Zones | Co-located VPC Zones   |
|--------------|--------------------|----------|------------------|--------------------------|------------------------|
| America      | Dallas, USA        | us-south | DAL12, us-south  | DAL12, DAL13             | eu-south-2, eu-south-3 |
| America      | Washington DC, USA | us-east  | us-east          | WDC04                    | us-east-1              |
| America      | São Paulo, Brazil  | br-sao   | SAO01            | SAO01                    |                        |
| America      | Toronto, Canada    | ca-tor   | TOR01            | TOR01                    |                        |
| America      | Montreal, Canada   | ca-mon   | MON01            | MON01                    |                        |
| Europe       | Frankfurt, Germany | eu-de    | eu-de-1, eu-de-2 | FRA04, FRA05             | eu-de-2, eu-de-3       |
| Europe       | London, UK         | eu-gb    | LON04, LON06     | LON04, LON06             | eu-gb-1, eu-gb-3       |
| Asia Pacific | Sydney, Australia  | au-syd   | SYD04            | SYD04                    | au-syd-2               |
| Asia Pacific | Tokyo, Japan       | jp-tok   | TOK04            | TOK04                    | jp-tok-2               |
| Asia Pacific | Osaka, Japan       | jp-osa   | OSA21            | OSA21                    |                        |

Because the network between POWER hardware and standard x86 infrastructure on IBM Cloud is physically decoupled, Power VS's can't access other x86 resources by default. You can optionally configure a Private Network interface and [Direct Link Connect for Power Systems VS](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-ordering-direct-link-connect) to enable the seamless connection between other IBM Cloud IaaS resources and Power VS instances.

### 1.4. Network options

When creating a Power VS you are able to select a Public or Private Network (and also configure subnets):

- Public VLANs:

    - Easy and quick method to connect to a Power Systems Virtual Server instance.
    - Public networks are IBM-provided and have an associated cost.
    - IBM configures the network environment to enable a secure public network connection from the internet to the Power Systems Virtual Server instance.
    - Connectivity is implemented by using an IBM Cloud Virtual Router Appliance (VRA) and a Direct Link Connect connection.
    - Protected by firewall and supports the following secure network protocols: SSH, HTTPS, Ping, and IBM i 5250 terminal emulation with SSL (port 992).
    
- Private VLANs:

    - Allows your Power Systems Virtual Server instance to access existing IBM Cloud resources, such as IBM Cloud Bare Metal Servers, Kubernetes containers, and Cloud Object Storage.
    - Uses a Direct Link Connect connection to connect to your IBM Cloud account network and resources.
    - Required for communication between different Power Systems Virtual Server instances.

If you need to connect to your virtual server through the public internet, in other words, inbound to a virtual server, you can order Public IPs and attach them to the virtual server per vNIC. Various interconnectivity options available, for example:

- IBM Power Systems Infrastructure bridged to IBM Cloud Classic Infrastructure
- IBM Power Systems Infrastructure bridged to IBM Cloud VPC Infrastructure
- IBM Power Systems Infrastructure bridged to on-premises data centers by using IBM Direct Link

IBM Cloud Direct Link on Classic must be used to connect your IBM Power Systems Virtual Servers with your IBM Cloud Classic Infrastructure and VPC Infrastructure resources. After you configure Direct Link on Classic, you must configure routing on your virtual server instance. Read more at [Configuring connectivity to Power Systems Virtual Server](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-configuring-power) and [Configuring and adding a private network subnet](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-configuring-subnet).

### 1.5. Storage options

The storage tiers in Power Systems Virtual Server are based on I/O operations per second (IOPS). It means that the performance of your storage volumes is limited to the maximum number of IOPS based on volume size and storage tier.

Within IBM Power Systems Infrastructure, there are two types of storage available:

| Type | I/O Operations per Second | Description |
|-|-|-|
| Block Storage Tier 1 | 10 IOPS/GB | Storage for mission critical application with best characteristics, e.g. NVMe flash storage |
| Block Storage Tier 3 | 3 IOPS/GB | Default storage type with optimized price/performance, e.g. SSD flash storage |

Although, the exact numbers might change over time, the Tier 3 storage is currently set to 3 IOPS/GB, and the Tier 1 storage is currently set to 10 IOPS/GB. For example, a 100 GB Tier 3 storage volume can receive up to 300 IOPs, and a 100 GB Tier 1 storage volume can receive up to 1000 IOPS. After the IOPS limit is reached for the storage volume, the I/O latency increases.

When creating a new Power VS instance, you can either create a new data volume or attach an existing one that you defined previously in your IBM Cloud account.

### 1.6. Compute options

Currently the following IBM Power Systems hardware are utilised by IBM Power VS:

- S922 – optimized for SAP NetWeaver application server
- E980 – optimized for SAP HANA database server
- E880

When creating a Power VS instance, you are able to choose any option from the three underlying machine profiles: S922, E980, E880 (E880 is only available at Dallas and Washington DC). The IBM Power Systems Virtual Servers are SAP-certified. For more information, see [Infrastructure certified for SAP](https://cloud.ibm.com/docs/sap?topic=sap-iaas-offerings).

You can select the number of cores and RAM memory when creating Power VS's instances. Dedicated Core Types have a core-to-vCPU ratio of 1:1. For shared processors, fractional cores round up to the neares whole number. For example, 1.25 cores equals 2 vCPUs. By default, all physical processors that are not dedicated to specific logical partitions (LPARs) are grouped together in a shared processor pool. You can assign a specific amount of the processing capacity in this shared processor pool to each logical partition that uses shared processors.

Logical partitions that use shared processors can have a sharing mode of `capped` or `uncapped`. An uncapped logical partition is a logical partition that can use more processor power than its assigned processing capacity. The amount of processing capacity that an uncapped logical partition can use is limited only by the number of virtual processors assigned to the logical partition or the maximum processing unit that is allowed by the shared processor pool that the logical partition uses. In contrast, a capped logical partition is a logical partition that cannot use more processor power than its assigned processing units.

You can also configure VM pinning strategies (`Off`, `Soft`, or `Hard`). VM pinning allows you to control the movement of VMs during disasters and other restart events. You can choose to soft pin or hard pin a VM to the host where it is running. When you soft pin a VM for high availability, PowerVC automatically migrates the VM back to the original host once the host is back to its operating state. If the VM has a licensing restriction with the host, the hard pin option restricts the movement of the VM during remote restart, automated remote restart and live partition migration. The default pinning policy is `Off`.

### 1.7. Available software

IBM Cloud has several boot images available for provisioning into IBM Power Systems Virtual Servers, including:

- AIX
- IBM i
- Linux (client supplied subscription)
- Linux for SAP Hana (client supplied subscription)
- Linux for SAP NetWeaver (client supplied subscription)

If deploying a Linux VM, a subscription must first be purchased and registered. Then, after deployment, register with your Linux vendor. Learn more about [purchasing and subscribing to Linux and requirements for SAP workloads](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-using-linux).

You can also bring your custom boot image to Power Systems Virtual Server.