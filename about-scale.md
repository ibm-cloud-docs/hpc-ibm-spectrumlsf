---

copyright:
  years: 2025
lastupdated: "2025-01-20"

keywords:
subcollection: hpc-ibm-spectrumlsf

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}

# About {{site.data.keyword.scale_full_notm}} with {{site.data.keyword.spectrum_full_notm}}
{: #about-scale}

The default shared file storage solution for your {{site.data.keyword.spectrum_full_notm}} cluster is {{site.data.keyword.filestorage_vpc_full}}, which can connect up to 256 hosts per zone per VPC. This limits the maximum number of dynamic compute nodes to 250. Instead, you can use {{site.data.keyword.scale_full}} as your storage solution, which can mount up to 4000 NFS connections per protocol and can be extended to a 32-node protocol cluster. If you use {{site.data.keyword.scale_short}} as your storage solution, you first [set up a {{site.data.keyword.scale_short}} cluster](https://cloud.ibm.com/catalog/content/ibm-spectrum-scale-d722b6b6-8bb5-4506-8f0f-03a5f05a3d6e-global), and then [integrate a list of values from the {{site.data.keyword.scale_short}} deployment with the {{site.data.keyword.spectrum_full_notm}} cluster deployment](/docs/allowlist/hpc-service?topic=hpc-service-integrating-scale).

{{site.data.keyword.scale_short}} is a clustered file system that provides concurrent access to a single file system or set of file systems from multiple nodes. It enables high-performance access to a common set of data to support a scale-out solution or to provide a high availability platform. {{site.data.keyword.scale_short}} can run on virtualized instances that provide common data access in environments, and uses logical partitioning or other hypervisors. Multiple {{site.data.keyword.scale_short}} clusters can share data within a location or across a wide area network (WAN) connections.

{{site.data.keyword.scale_short}} is a file system that is defined over one or more nodes. On each node in the cluster, {{site.data.keyword.scale_short}} includes three components: administration commands, a kernel extension, and a multithreaded daemon.

Also, you can use {{site.data.keyword.scale_short}}'s single system to share dedicated file systems for data storage. All of your {{site.data.keyword.cloud_notm}} dynamic nodes can access the shared data throughout the lifecycle of the cluster through [Cluster Export Services (CES)](/docs/storage-scale?topic=storage-scale-config-ces-integration-ldap-authentication) NFS mount points. CES is part of the {{site.data.keyword.scale_full_notm}} architecture, and enables access to your data. When you deploy an {{site.data.keyword.scale_full_notm}} cluster, you also configure CES deployment values so that {{site.data.keyword.scale_short}} and {{site.data.keyword.spectrum_full_notm}} can share data.

For more information, see the [{{site.data.keyword.scale_short}} documentation](/docs/storage-scale?topic=storage-scale-getting-started-tutorial).
{: shortdesc}
