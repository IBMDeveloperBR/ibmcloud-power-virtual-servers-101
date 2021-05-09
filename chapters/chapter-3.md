# IBM Power Systems Virtual Servers on IBM Cloud

[![service-card](../images/service-card.png)](https://cloud.ibm.com/catalog/services/power-systems-virtual-server)

## 3. High Availability and Disaster Recovery options for IBM Power VS

The Power Systems Virtual Server instance restarts the virtual servers on a different host system if a hardware failure occurs. This process provides basic High Availability (HA) capabilities for the Power Systems Virtual Server service. If you want more advanced HA or Disaster Recover (DR) solutions, you can deploy the following applications in your Power Systems Virtual Server environment:

- **High Availability with PowerHA SystemMirror for AIX**

You can use a monthly subscription model when you purchase PowerHA SystemMirror for AIX Standard Edition. After purchasing, you can download PowerHA SystemMirror from [Entitled Software Support (ESS)](https://www.ibm.com/servers/eserver/ess/index.wss). You can install PowerHA SystemMirror for AIX on the virtual server that is running in your Power Systems Virtual Server environment.

Review the following information for implementing PowerHA SystemMirror for AIX in your Power Systems Virtual Server environment.

1. When you are creating the virtual servers that are part of the PowerHA SystemMirror cluster, you must select Different Server from the Colocation Rules field. Selecting Different Server ensures that the different logical partitions (LPARs) that will be a part of the PowerHA SystemMirror cluster are not deployed on the same host.

2. You must select `On` from the Shareable field when you create storage volumes for the virtual severs that are part of the PowerHA SystemMirror cluster.

3. By using the Power Systems Virtual Server service, you do not have access to the HMC, VIOS, and the host system. Therefore, any PowerHA SystemMirror functions that require access to these capabilities, such as Resource Optimized High Availability (ROHA) and Active Node Halt Policy (ANHP), are not available.

- **Disaster Recovery mechanisms for AIX and IBM i**

There are also some Disaster Recovery mechanisms available specifically for AIX and IBM i VMs.

You can implement disaster recovery mechanism between two AIX virtual server instances in separate IBM Cloud data centers by using GLVM replication. For a complete tutorial, see [AIX Disaster Recovery with IBM Power Systems Virtual Servers](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_AIX_DR_Tutorial_v1.pdf).

You can implement disaster recovery mechanisms between two IBM i virtual server instances by using PowerHA geographic mirroring. [For a complete tutorial, see IBM i Disaster Recovery with IBM Power Systems Virtual Servers](https://cloud.ibm.com/media/docs/downloads/power-iaas-tutorials/PowerVS_IBMi_DR_Tutorial_v1.pdf).

- **Useful resources**

    - [PowerHA SystemMirror for AIX Standard Edition monthly pricing options](https://www-01.ibm.com/common/ssi/ShowDoc.wss?docURL=/common/ssi/rep_ca/8/897/ENUS219-288/index.html&request_locale=en)
    - [Installing PowerHA SystemMirror](https://www.ibm.com/support/knowledgecenter/SSPHQG_7.2/install/ha_install.html)
    - [Official Power Virtual Servers on IBM Cloud documentation](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-ha-dr)