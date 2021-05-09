# IBM Power Systems Virtual Servers on IBM Cloud

[![service-card](../images/service-card.png)](https://cloud.ibm.com/catalog/services/power-systems-virtual-server)

## 2. Backup and migration strategies for IBM Power VS

### 2.1. Backup strategies

- **Image Capture**

You can use image capture to store VM images within your account as part of the image catalog or directly to IBM Cloud Object Storage (COS). This method works for Linux, AIX and IBM i systems on Power Virtual Servers.

Because importing and exporting images requires a considerable amount of processing power and network bandwidth, you can submit only one import or export request before it is queued.

The maximum image size that you can import or export with image capture is 10TB.

- **AIX backup strategies**

Power Systems Virtual Server users can also implement any compatible agent-based backup for AIX virtual machines. Veeam and IBM Spectrum Protect are the most commonly used backup strategies.

1. Veeam for AIX: https://www.veeam.com/ibm-aix-oracle-solaris-backup.html

2. IBM Spectrum Protect: https://www.ibm.com/products/data-protection-and-recovery

- **IBM i backup strategies**

A common IBM i backup strategy is to use *IBM Backup, Recovery, and Media Services (BRMS)* and *IBM Cloud Storage Solutions (ICC)*. Together, these products automatically back up your LPARs to IBM Cloud Object Storage. The ICC product can be integrated with BRMS to move and retrieve objects from remote locations, including COS. In most cases, this process involves backing up to virtual tapes and image catalogs. You might need extra storage for the LPAR to host the image catalogs until they are moved to COS.

The typical IBM i customer uses the following flow to perform a backup:

1. Use the 5733-ICC product to connect to COS (~2 times the disk capacity to hold the backup images).
2. Connect to IBM's classic infrastructure by using Direct Link Connect.
3. Create a Nginx reverse proxy. You can provision the reverse proxy from a virtual server instance (x86) for bandwidth up to 1 Gbps. If more bandwidth is needed, you must use a bare metal server.
4. Complete the back up to COS by choosing the speed and resiliency that is required.

Useful resources:

i. [Working with ICC](https://www.ibm.com/support/knowledgecenter/ssw_ibm_i_72/icc/topics/iccucon_commands_cloud_overview.htm)
ii. [BRMS with Cloud Storage Solutions for i considerations and requirements](https://www.ibm.com/support/knowledgecenter/en/ssw_ibm_i_74/rzai8/rzai8brmscloudrequireandconsider.htm)
iii. [Data backup and recovery by using BRMS and IBM Cloud Storage Solutions for i](https://www.ibm.com/support/knowledgecenter/en/ssw_ibm_i_74/rzai8/rzai8backupandrecoveryusingBRMSandICC.htm)

- **Using COS over IBM Cloud Direct Link**

IBM Cloud customers who purchase COS and Direct Link can make remote connections to COS private endpoints. This type of connection extends the advantages of private service endpoints, so they can be used by client systems outside of IBM Cloud facilities.

HTTPS (secure HTTP) COS requests are initiated from a client at a remote site. They're transmitted securely through IBM Cloud Direct Link, targeting one of a cluster of reverse proxy servers deployed in a customer’s IBM Cloud account. From there, requests are passed to a COS private endpoint, processed, and then the results returned to the remote calling client.

### 2.2. Migration strategies

- **IBM Cloud Object Storage**

COS can be used as an intermediary location to store files from your on-premises environment. You can retrieve and send your files to the Power Systems Virtual Server environment from this location. You must create COS buckets to transfer data over the public internet and or privately secured links. [For more information, see IBM Cloud Object Storage: FAQ](https://www.ibm.com/cloud/object-storage/faq).

To copy data from COS to your AIX virtual machine (VM), you must install the Amazon Web Services (AWS) CLI by using either *yum* or *pip*. If you use the yum package manager, you can install the AWS CLI with the `yum install aws-cli` command. For the pip package manager, you can use `pip install awscli` to install the AWS CLI. After the installation, you can use the universal S3 commands that are supported by AWS to copy objects.

You can also find a script that ships (as a sample command) with AIX 7.2 TL3, or later, that simplifies the installation of yum, pip, and the AWS CLI. You can find the script at: `/usr/samples/nim/cloud_setup`.

Useful resources:

- [Configuring YUM and creating local repositories on IBM AIX](https://developer.ibm.com/technologies/systems/articles/configure-yum-on-aix/)
- [Python for AIX](http://www.aixtools.net/index.php/python)

For an IBM i VM, you must use the [Cloud Storage Solution for IBM i product (5733-ICC)](https://www.ibm.com/support/pages/ibm-cloud-storage-solutions-i) to communicate with COS and transfer data.

- **Mass Data Migration (MDM)**

MDM provides a simple and secure way to physically transfer data (terabytes to petabytes) to the IBM Cloud Object Storage to be used by Power Systems Virtual Server. As part of the MDM process, IBM sends the client an MDM-approved device, who uploads their on-premises data to the device and sends it back. IBM then transfers and stores the content in COS for later retrieval from within the Power Systems Virtual Server environment. [To learn more, see Mass Data Migration: FAQ](https://www.ibm.com/cloud/mass-data-migration/faq).

The data transfer rate for MDM on an IBM i system is roughly 110-120 MB/sec. It takes between 2.5 to 3 hours to successfully transfer 1 TB of data.

- **PowerVC images and COS**

If you have an environment with access to PowerVC, you can capture OVA images to easily migrate your data. The Power Systems Virtual Server offering allows you to provision a new virtual server based on an OVA image. To accomplish this, regardless of the operating system (OS), you must complete the following steps:

1. Create an OVA image on your local system.
2. Copy the OVA image to your Cloud Object Storage account.
3. Deploy the OVA image and provision a new Power Systems Virtual Server.

- **OS-based and application specific replication strategies**

You can use various replication mechanisms to migrate and sync environments between your on-premises environment and the Power Systems Virtual Server.

For host and OS-based logical replication, IBM recommends that you use PowerHA SystemMirror (Enterprise Edition) with Geographic Mirroring (Geomirroring) and Geographic Logical Volume Manager (GLVM). The following host and OS-based logical replication options exist depending on your OS:

1. AIX - GLVM
2. IBM i - Geomirroring

Applications might have replication mechanisms that can sync multiple environments. These options are commonly used for application-specific replication:

1. Db2 HADR
2. Oracle Data Guard
3. Oracle Goldengate
4. iCluster
5. MIMIX from Syncsort

You can read more about specific AIX and IBM i migration strategies in the official docs:

i. [IBM AIX migration strategies](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-migration-strategies-power#migration-aix)
ii. [IBM i migration strategies](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-migration-strategies-power#migration-ibmi)

### 2.3. Planning workload migrations to POWER8 and POWER9

When workloads are deployed on a new system, you must pay attention to its configuration and tuning to achieve the expected performance. The Power Systems Virtual Server service uses three different IBM Power Systems: E880 (9119-MHE), E980 (9080-M9S), and S922 (9009-22A). [For more information, see Hardware specifications](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-about-virtual-server#hardware-specifications).

The Power Systems Virtual Server service supports only AIX 7.1, or later. If you use an unsupported version, it is subject to outages during planned maintenance windows with no advanced notification given. Your current AIX level and POWER processor family can help determine which migration path to follow.

IBM i customers must use IBM i 7.1, or later. Clients running IBM i 6.1 must first upgrade the operating system (OS) to a current support level before migrating to the Power Systems Virtual Server. IBM i 7.2 supports direct upgrades from IBM i 6.1 or 7.1 (N-2).
Migration checklist

Before you migrate to a newer IBM Power System, review the following checklist:

1. Plan the system migration.
2. Install the latest required software and apply the available fixes.
3. Set the appropriate processor compatibility mode for logical partitions (LPARs) before and after migration.
4. Plan the virtual processor (VP) and entitlement for each LPAR to best fit your operation and performance requirements.
5. Follow the I/O consideration guide.
6. Consider contacting [IBM Systems Lab Services](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-system-migration#lab-services) to ease the migration process.
