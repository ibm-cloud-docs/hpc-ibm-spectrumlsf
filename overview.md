---

copyright:
  years: 2025
lastupdated: "2025-01-21"

keywords: 

subcollection: hpc-ibm-spectrumlsf

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Overview of IBM Spectrum LSF
{: #about-spectrum-lsf}

{{site.data.keyword.spectrum_full} is a scheduling software to enable High Performance Computing (HPC) clusters. This offering uses deployable architecture to provision and configure {{site.data.keyword.cloud_notm}} resources. With simple steps to define configuration properties and use automated deployment, you can build your own HPC clusters in minutes by using your choice of an Intel x86 based [VPC virtual server instance profile type](/docs/vpc?topic=vpc-profiles&interface=ui) for the worker nodes in the cluster. {{site.data.keyword.spectrum_full}} also enables configuration for auto scaling, so {{site.data.keyword.spectrum_full}} clusters can automatically add and remove worker nodes based on workload specifications. This allows to take full advantage of consumption-based pricing and pay for cloud resources only when they are needed. 
{: shortdesc}

{{site.data.keyword.spectrum_full_notm}} offers the option of a public virtual machine, or virtual machines that are deployed on dedicated hosts, for static compute nodes only. The management nodes and dynamic compute nodes use public virtual machines only. The dedicated host option allows you to have systems that are assigned just for your workloads and avoids issues like a noisy neighbor. You can pack a dedicated host to full capacity before spilling to another instance or spread the virtual server instances evenly across all dedicated hosts.

In addition, {{site.data.keyword.spectrum_full}} provides two shared storage options for you to manage your application data: File storage for VPC or {{site.data.keyword.scale_short}}.

The {{site.data.keyword.scale_short}} feature is specifically designed to work with {{site.data.keyword.spectrum_full}} cluster nodes. To leverage this functionality, customers must first deploy an IBM {{site.data.keyword.scale_short}} cluster with CES enabled as a prerequisite. Once set up, CES-based NFS mount points can be exported to the LSF cluster as shared mount points. This integration allows the LSF cluster to access the same mount points and share data with the Storage Scale cluster, enabling the deployment of a high-performance file system within your HPC cluster.

The offering supports the bring-your-own-license (BYOL) model for [{{site.data.keyword.spectrum_full_notm}}](https://www.ibm.com/products/hpc-workload-management){: external} to deploy an HPC cluster on {{site.data.keyword.cloud_notm}}. Make sure that you have sufficient software licenses to deploy the required capacity on the {{site.data.keyword.cloud_notm}} cluster. For evaluation purposes, {{site.data.keyword.cloud_notm}} does enable limited access. Contact your {{site.data.keyword.cloud_notm}} sales or support team for evaluation licenses.

{{site.data.keyword.spectrum_short}} enables all three interfaces: UI, API, and CLI. To use the API and CLI interfaces, the Terraform-based automation code is available in this [public GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-hpc){: external}.

{{site.data.keyword.spectrum_full}} also offers the [LSF Application Center](https://www.ibm.com/docs/en/slac/10.2.0){: external}, which provides a flexible, easy-to-use interface for cluster users and administrators. Available as an add-on module to IBM Spectrum LSF, the LSF Application Center enables users to interact with intuitive, self-documenting, standardized interfaces. You can access the LSF Application Center through the [GUI](/docs-draft/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-accessing-gui&interface=ui), and you can also access the API calls with [Python](/docs-draft/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-access-rest-api-calls-pacclient&interface=ui) and [`curl`](/docs-draft/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-access-rest-api-calls-curl&interface=ui).

The LSF cluster is configured not only with the Application Center feature but also with the Application Center High Availability (HA) functionality. In the event of a failover, the PAC feature will remain operational, ensuring users can still access and interact with the GUI. Jobs will continue running as long as at least one LSF management host is available. The cluster deploys GUI services across three GUI servers, with the database hosted on one of these GUI hosts.

The offering enables the initial Spectrum LSF-based HPC cluster creation. Any updates that are needed post-deployment regarding LSF configuration or setup must be performed by using LSF tools and commands. If you use the {{site.data.keyword.bpshort}} interface to change configuration properties and reapply those changes, you can cause disruptions to the running {{site.data.keyword.spectrum_short}} cluster. Restoring it back to a working state might not be easy.
{: important}
