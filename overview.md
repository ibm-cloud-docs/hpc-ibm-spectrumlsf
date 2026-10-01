---

copyright:
  years: 2026
lastupdated: "2026-07-16"

keywords:

subcollection: hpc-ibm-spectrumlsf
use-case: ITServiceManagement
industry: Technology

deployment_url: https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-hpc-lsf-1444e20a-af22-40d1-af98-c880918849cb-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2cjaGlnaGxpZ2h0cw%3D%3D
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Overview of IBM Spectrum LSF
{: #about-spectrum-lsf}
{: toc-industry="Technology"}
{: toc-use-case="ITServiceManagement"}

{{site.data.keyword.spectrum_full}} is a scheduling software that enables High-Performance Computing (HPC) clusters on {{site.data.keyword.cloud_notm}}. This offering uses deployable architecture to provision and configure {{site.data.keyword.cloud_notm}} resources, allowing you to build HPC clusters in minutes.
{: shortdesc}

With simple configuration steps and automated deployment, you can create your own HPC clusters by using your choice of an Intel x86 based [VPC virtual server instance profile type](/docs/vpc?topic=vpc-profiles&interface=ui) for the worker nodes in the cluster. {{site.data.keyword.spectrum_short}} also supports auto scaling, enabling clusters to automatically add and remove worker nodes based on workload specifications. This capability allows you to take full advantage of consumption-based pricing and pay for cloud resources only when they are needed.

## Key features
{: #key-features}

IBM Spectrum LSF on {{site.data.keyword.cloud_notm}} provides the following key features:

Deployable architecture
:   Uses modular components and dependencies for seamless deployment, enabling quick feature updates without extensive manual intervention. For more information, see [Deployable architectures](https://www.ibm.com/think/insights/deployable-architecture-on-ibm-cloud-simplifying-system-deployment){: external}.

Flexible compute options
:   Supports both public virtual systems and private virtual systems deployed on dedicated hosts for static compute nodes. Management nodes and dynamic compute nodes use public virtual machines only.

Shared storage options
:   Provides two storage solutions to manage application data:
    - File storage for VPC
    - Storage Scale integration

Bring-your-own-license (BYOL) model
:   Supports BYOL for [IBM Spectrum LSF](https://www.ibm.com/products/hpc-workload-management){: external}, allowing you to use your existing licenses.

Multiple interfaces
:   Accessible through UI, API, and CLI interfaces.

## Dedicated hosts for virtual server instances
{: #dedicated-hosts}

For static compute environments, IBM Spectrum LSF provides dedicated host capabilities ensuring exclusive access to compute resources and minimizing the impact of noisy neighbors. You can configure dedicated hosts to:

- Pack a dedicated host to full capacity before using another instance.
- Spread virtual server instances evenly across all dedicated hosts.

For more information, see [Dedicated Hosts for Virtual Server Instances](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-dedicated-hosts-vsi).

## Storage Scale integration
{: #storage-scale}

The Storage Scale feature is designed to work with Spectrum LSF cluster nodes. To use this functionality:

1. Deploy an IBM Storage Scale cluster with CES enabled as a prerequisite.
2. Export CES-based NFS mount points to the LSF cluster as shared mount points.

This integration allows the LSF cluster to access the same mount points and share data with the Storage Scale cluster, enabling a high-performance file system within your HPC cluster.

## LSF Application Center
{: #lsf-application-center}

IBM Spectrum LSF includes the [LSF Application Center](https://www.ibm.com/docs/en/slac/10.2.0){: external}, an add-on module that provides a flexible, easy-to-use interface for cluster users and administrators. The Application Center enables users to interact with intuitive, self-documenting, and standardized interfaces.

You can access the LSF Application Center through:

- The GUI interface
- API calls with [Python](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-access-rest-api-calls-pacclient&interface=ui)
- API calls with [`curl`](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-access-rest-api-calls-curl&interface=ui)

### High availability
{: #high-availability}

The LSF cluster is configured with Application Center High Availability (HA) functionality. In the event of a failover:

- The Application Center feature remains operational.
- Users can continue to access and interact with the GUI.
- Jobs continue to run as long as at least one LSF management host is available.

The cluster deploys GUI services across three GUI servers, with the database hosted on one of these GUI hosts.

## LSF Web Services
{: #lsf-web-services}

IBM Spectrum LSF Web Services provides a RESTful HTTP(s) interface for remotely interacting with LSF clusters. This feature, introduced in Fix Pack 15, enables you to:

- Programmatically submit, monitor, and manage jobs.
- Integrate LSF workload management into custom applications, web portals, and automation workflows.
- Access the cluster securely through standard APIs without requiring direct command-line access.

## Licensing requirements
{: #licensing-requirements}

The current solution no longer requires an IBM Customer Number (ICN) for entitlement checks before deploying the solution for non-production use. You can provision up to a maximum of 10 static worker nodes for evaluation or non-production use cases without ICN validation.

If the number of worker nodes exceeds 10, you are responsible for obtaining the necessary entitlement check and licensing for those additional nodes in the production environment. For production use or for evaluating more than 10 worker nodes, you must purchase the necessary LSF licenses. To purchase licenses, see [Purchasing licenses](https://www.ibm.com/docs/en/devops-test-embedded/9.0.0?topic=licenses-purchasing){: external}.
{: important}

Make sure that you have sufficient software licenses to deploy the required capacity on the {{site.data.keyword.cloud_notm}} cluster. For evaluation purposes, {{site.data.keyword.cloud_notm}} enables limited access. Contact your {{site.data.keyword.cloud_notm}} sales or support team for evaluation licenses.

## Post-deployment considerations
{: #post-deployment}

The offering enables the initial Spectrum LSF-based HPC cluster creation. Any updates needed post-deployment regarding LSF configuration or setup must be performed using LSF tools and commands.

If you use the Schematics interface to change configuration properties and reapply those changes, you can cause disruptions to the running Spectrum LSF cluster. Restoring it back to a working state might not be easy.
{: important}

## Next steps
{: #next-steps}

- [Planning your deployment](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-planning-deployment)
- [Getting started with IBM Spectrum LSF](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-getting-started)
- [Configuring auto scaling](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-configure-auto-scaling)
