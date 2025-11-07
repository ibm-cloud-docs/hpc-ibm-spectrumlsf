---

copyright:
  years: 2025
lastupdated: "2025-10-07"

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

{{site.data.keyword.spectrum_full}} is a scheduling software to enable High-Performance Computing (HPC) clusters. This offering uses deployable architecture to provision and configure {{site.data.keyword.cloud_notm}} resources. With simple steps to define configuration properties and use automated deployment, you can build your own HPC clusters in minutes by using your choice of an Intel x86 based [VPC virtual server instance profile type](/docs/vpc?topic=vpc-profiles&interface=ui) for the worker nodes in the cluster. {{site.data.keyword.spectrum_short}} also enables configuration for auto scaling, so {{site.data.keyword.spectrum_short}} clusters can automatically add and remove worker nodes based on workload specifications. This allows you to take full advantage of consumption-based pricing and pay for cloud resources only when they are needed. 
{: shortdesc}

A deployable architecture involves components, modules, and dependencies in a way that allows for seamless deployment and makes it easy for developers and operations teams to quickly deploy new features and updates to the system, without requiring extensive manual intervention. Refer the [Deployable architecture](https://www.ibm.com/think/insights/deployable-architecture-on-ibm-cloud-simplifying-system-deployment) document for more detailed information.

For static compute nodes, {{site.data.keyword.spectrum_full_notm}} offers the option of public virtual system or private virtual system, that are deployed on dedicated hosts. The management nodes and dynamic compute nodes use public virtual machines only. The dedicated host option allows you to have systems that are assigned just for your workloads and avoids issues like a noisy neighbor. You can pack a dedicated host to full capacity before spilling to another instance or spread the virtual server instances evenly across all dedicated hosts. Go to [Dedicated Hosts for Virtual Server Instances](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-dedicated-hosts-vsi) for more information.

In addition, {{site.data.keyword.spectrum_short}} provides two shared storage options to manage your application data:
* File storage for VPC or
* {{site.data.keyword.scale_short}}

The {{site.data.keyword.scale_short}} feature is designed to work with {{site.data.keyword.spectrum_short}} cluster nodes. To leverage this functionality, users must first deploy an IBM {{site.data.keyword.scale_short}} cluster with CES enabled as a prerequisite. Once set up, CES-based NFS mount points can be exported to the LSF cluster as shared mount points. This integration allows the LSF cluster to access the same mount points and share data with the Storage Scale cluster, enabling the deployment of a high-performance file system within your HPC cluster.

The offering supports the bring-your-own-license (BYOL) model for [{{site.data.keyword.spectrum_full_notm}}](https://www.ibm.com/products/hpc-workload-management){: external} to deploy an HPC cluster on {{site.data.keyword.cloud_notm}}. Make sure that you have sufficient software licenses to deploy the required capacity on the {{site.data.keyword.cloud_notm}} cluster. For evaluation purposes, {{site.data.keyword.cloud_notm}} does enable limited access. Contact your {{site.data.keyword.cloud_notm}} sales or support team for evaluation licenses.

The {{site.data.keyword.spectrum_short}} enables all three interfaces: UI, API, and CLI.

{{site.data.keyword.spectrum_full_notm}} also offers the [LSF Application Center](https://www.ibm.com/docs/en/slac/10.2.0){: external}, which provides a flexible, easy-to-use interface for cluster users and administrators. It is available as an add-on module to {{site.data.keyword.spectrum_full_notm}}, the LSF Application Center enables users to interact with intuitive, self-documenting, standardized interfaces. You can access the LSF Application Center through the GUI, and you can also access the API calls with [Python](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-access-rest-api-calls-pacclient&interface=ui) and [`curl`](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-access-rest-api-calls-curl&interface=ui).

The LSF cluster is configured not only with the Application Center feature but also with the Application Center High Availability (HA) functionality. In the event of a failover, the PAC feature remains operational, ensuring that users can still access and interact with the GUI. Jobs continue to run as long as at least one LSF management host is available. The cluster deploys GUI services across three GUI servers, with the database hosted on one of these GUI hosts.

IBM Spectrum LSF Process Manager is a component of the IBM Spectrum LSF (Load Sharing Facility) suite that provides advanced workload scheduling and job management. This helps users to automate, monitor, and control application workflows and dependencies across distributed computing environments. With process manager, users can define complex job flows using visual tools or scripting, enabling efficient resource usage and better throughput. For more information, see [IBM Spectrum LSF Process Manager](/docs/hpc-ibm-spectrumlsf?group=ibm-spectrum-lsf-process-manager) documentation.

IBM Spectrum LSF Web Services provides a RESTful HTTP(s) interface for remotely interacting with LSF clusters. This helps users to programmatically submit, monitor, and manage jobs, enabling seamless integration of LSF workload management into custom applications, web portals, and automation workflows. Spectrum LSF Web Services is introduced in Fix Pack 15. This eliminates the need for direct command-line access and offers secure, scalable access through standard APIs.

The offering enables the initial Spectrum LSF-based HPC cluster creation. Any updates that are needed post-deployment regarding LSF configuration or setup must be performed by using LSF tools and commands. If you use the {{site.data.keyword.bpshort}} interface to change configuration properties and reapply those changes, you can cause disruptions to the running {{site.data.keyword.spectrum_short}} cluster. Restoring it back to a working state might not be easy.
{: important}

The current solution no longer requires `ibm_customer_number`(ICN) for entitlement check before deploying the solution for non-production use. The solution is now available for use without ICN validation. Users can provision up to a maximum of 10 static worker nodes for evaluation or non-production use cases. If the number of worker nodes exceeds 10, it becomes the user responsibility to obtain the necessary entitlement check and licensing for those additional nodes in the production environment. For production use or for evaluating greater than 10 worker nodes, the user must purchase the necessary LSF licenses. To purchase the license, go to [Purchasing licenses](https://www.ibm.com/docs/en/devops-test-embedded/9.0.0?topic=licenses-purchasing).
{: important}
