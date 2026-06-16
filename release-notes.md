---

copyright:
  years: 2026
lastupdated: "2026-06-16"

keywords: IBM Spectrum LSF release notes

subcollection: hpc-ibm-spectrumlsf

content-type: release-note

---



{{site.data.keyword.attribute-definition-list}}
{:external: target="_blank" .external}
{:release-note: data-hd-content-type='release-note'}



# Release notes
{: #my-service-relnotes}

The release notes describes the brief overview of the new features, enhancements, known and fixed issues added to {{site.data.keyword.spectrum_full}} for the release.
{: shortdesc}

**For this release, the {{site.data.keyword.spectrum_full_notm}} version is 3.3.1**

## June 2026
{: #subcollection-june26}

### 19 June 2026
{: #subcollection-june2026}
{: release-note}

Following are the changes made for the release:

* Currently, we are supporting "IBM Spectrum LSF" solution. As part of our upcoming quarterly release, we are introducing support for **Bare metal infrastructure provisioning** for our compute nodes.

    * Bare metal does not support LSF Pay-As-You-Go (PAYGo) model.
    * When bare metal is enabled, the dedicated host option is not supported.

* Previously, our platform supported only Virtual Server Instances (VSI). With this release, we support both bare metal and VSI environments, providing greater flexibility for our customers. A combination of bare metal and virtual server instances (VSI) are not supported.

* Granite Rapids (Gen 4) profiles are supported as part of this as well.

* [SSD Defined Performance (SDP)](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-sdp-overview#dynamic): The SSD Defined Performance (SDP) is a second-generation IBM Cloud boot volume profile that provides enhanced flexibility in defining performance and capacity characteristics for boot volumes. By using the `sdp` profile, you can specify the boot volume capacity and configure the maximum throughput limit.

* [Spot Instances](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-si-overview): Spot instances offer a cost-efficient option for running transient and fault-tolerant workloads on IBM Cloud. They utilize unused cloud capacity and are available at significantly lower prices compared to standard virtual server instances.

* Dedicated host

## March 2026
{: #subcollection-mar26}

### 20 March 2026
{: #subcollection-mar2026}
{: release-note}

Support for AMD Turin profiles
:   The solution already supported Intel compute profiles, and now AMD Turin profiles are also supported. The only supported AMD-based profiles is **hx4da-248x680**.

Support for Intel Gaudi 3 compute profiles
:   Intel Gaudi 3 compute profiles are designed to accelerate large-scale AI and deep learning workloads within the HPC environments. Gaudi3 profiles enable efficient training and fine-tuning of Large Language Models (LLMs) across high-performance computing clusters.

Update the LSF configuration files to reflect the IBM Cloud hardware
:   The LSF software configurations are updated to detect and apply the appropriate computing hardware configurations, ensuring that applications run optimally based on both hardware and software characteristics.

Support for IBM solutions in the IBM Standard Edition
:   The LSF solution has been reverted to use the IBM Standard Edition software, as previously the solution relied on the Enterprise edition.

Support for LSF License Scheduler
:   The LSF License Scheduler manages license tokens instead of controlling the licenses directly.Using LSF License Scheduler, jobs obtain a license token before the application starts.

Observability modules
:   The observability modules have been updated to the latest version.

FP14 is no longer supported
:   In the current release, the solution does not support Fix Pack 14 (FP14), even though it was included as a supported feature in previous release.

SSH keys authorized only for Linux default user accounts
:   In compliance with IBMs SSH policy, automation disables root user access on all newly provisioned instances. Therefore, customers are required to access instances using the `lsfadmin` account as root login is not permitted.

## November 2025
{: #subcollection-nov25}

### 27 November 2025
{: #subcollection-nov2725}
{: release-note}

Support for LSF Pay-As-You-Go (PAYGo) feature
:  The LSF Pay-As-You-Go images are prebuilt virtual machine images available through the IBM Cloud Catalog. For more information, see [LSF Pay-As-You-Go (PAYGo) model](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-payg-model-intro).

### 07 November 2025
{: #subcollection-nov0725}
{: release-note}

Support for Web Services
:  IBM Spectrum LSF Web Services is enabled by default as part of the LSF suite deployment. It provides a secure RESTful interface to submit, monitor, and manage jobs, enabling easy integration with custom applications and automation workflows.

**Bug Fixes:**

* Added support for the "icgen2host" tag on dynamic nodes.
* Enabled deployment and configuration of core services (Application Center, Process Manager, Web Services, etc.) on Management Node-2 when the management node count is ≥ 2.

## September 2025
{: #subcollection-sep25}

### 18 September 2025
{: #subcollection-sep1825}
{: release-note}

Create IAM permissions using the CLI approach
:  Before deploying an {{site.data.keyword.spectrum_full_notm}} cluster, specific IAM permissions must be assigned to either a user or an access group. The automation script enables this process. For more information, see [Setting IAM permissions - CLI](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-getting-started-tutorial#setting-iam-permissions).

Support for Small Medium Large Deployments
:  This solution allows users to select the deployment options using three different t-shirt sizes - Small, Medium, and Large. For more information, see [Small Medium Large Deployments](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-sml-intro).

## July 2025
{: #subcollection-june25}

### 03 July 2025
{: #subcollection-july0325}
{: release-note}

**For this release, the DA tile version is 3.0.0**

Following are the change updates made for this release:

Support for Fix Pack 15 (FP15)
:  For this release, **Fix Pack 15** is supported. This fix pack contains all the security fixes, vulnerabilities fixes, resource connectors and so on.

Support for Process manager
:  IBM Spectrum LSF Process Manager is enabled by default as part of the LSF suite deployment. This helps users to automate, monitor, and control application workflows and dependencies across distributed computing environments.

Support for deployment using stock image
:  For this release, along with the custom image, user can do the deployment using the **stock image** also. The stock images can be used only for management and compute nodes and not for the deployer and dynamic nodes.

Updated the DA `landing_zone` and `landing_zone_vsi` module version.
:  The DA module versions for the landing zones are updated.

Support for Security and Compliance Center (SCC) Workload Protection
:  For this release, previously used SCC instances are deprecated and the new version of **SCC Workload Protection** is supported. Cloud-Native Application Protection Platform solution to manage your security and compliance posture, allowing you to monitor misconfigurations and detect and respond to vulnerabilities and threats in real-time.

Support for Deployer node
:  For this release, the entire cluster deployment is handled by the **deployer** node.

End to end deployment done through Ansible playbooks
:  Previously, all the configurations for {{site.data.keyword.spectrum_short}} were done using the user data through shell script. Now the cluster deployments and configurations are managed and handled by **Ansible** playbooks.

Application centre option is enabled by default for FP15
:  From the previous release, user had a choice to enable or disable the application centre feature as an option. For this release, **application centre** option is enabled by **default** for FP15.

## April 2025
{: #subcollection-apr25}

### 15 April 2025
{: #subcollection-apr1525}
{: release-note}

Following are the breaking change updates made for the release:
:  * All the VNI’s are placed under the same resource groups.
:  * Updated the DA `landing_zone` and `landing_zone_vsi` module version along with the IBM provider version.
:  * Fixed the custom image builder bug, to support custom image deployment through Mac and Linux based VSI.

## March 2025
{: #subcollection-mar25}

### 24 March 2025
{: #subcollection-mar2425}
{: release-note}

Bug Fix
:  To support the removal of ICN feature, created a new custom image for the creation of PAC.

### 07 March 2025
{: #subcollection-mar0725}
{: release-note}

IBM Customer Number (ICN) is not supported.
:  The current solution no longer requires `ibm_customer_number`(ICN) for entitlement check before deploying the solution for non-production use. The solution is now available for use without ICN validation. Users can provision up to a maximum of 10 static worker nodes for evaluation or non-production use cases. If the number of worker nodes exceeds 10, it becomes the user responsibility to obtain the necessary entitlement check and licensing for those additional nodes in the production environment. For production use or for evaluating greater than 10 worker nodes, the user must purchase the necessary LSF licenses. To purchase the license, go to [Purchasing licenses](https://www.ibm.com/docs/en/devops-test-embedded/9.0.0?topic=licenses-purchasing).

### 05 March 2025
{: #subcollection-mar0525}
{: release-note}

:  In this release, IBM Spectrum LSF deployable architecture is introduced. Spectrum LSF is a scheduling software to enable High-Performance Computing (HPC) clusters. This offering uses deployable architecture to provision and configure {{site.data.keyword.cloud_notm}} resources. For more information, refer [Overview of IBM Spectrum LSF](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-about-spectrum-lsf).

### What's New
{: #what-new}

The following new features are added as part of this release:

* [IBM Spectrum LSF deployable architecture](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-ibm-spectrum-lsf): Spectrum LSF enables High-Performance Computing (HPC) clusters by using LSF as the HPC scheduling software. This solution employs a deployable architecture to provision and configure IBM Cloud resources.
* [IBM Cloud Activity Tracker Event Routing](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-activity-tracker-overview): This is a platform service which manages the auditing events at the account-level by configuring targets and routes that define where auditing data is routed.
* [Multiple static worker node profile support](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-considerations-for-HPC-custer-compute-types): This solution supports the creation of static compute nodes by leveraging different instance type profiles based on resource requirements.
* [PAC High Availability (HA) support](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-accessing-gui): This allows LSF Application Center to run on all deployed management nodes, use a cross availability zone instance of the IBM Cloud® Database for MySQL as the backend database, and use an IBM Cloud® Application Load Balancer for VPC (ALB) as the VPC load balancer.
* [IBM Storage Scale support](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-integrating-scale): By using Storage Scale as your storage solution, you first set up a Storage Scale cluster, and then integrate a list of values from the Storage Scale deployment with the IBM Spectrum LSF cluster deployment. This provides more performance and scalability than standard file storage solutions.
* [IBM Cloud Monitoring](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-cloud-monitoring-overview): This is a cloud-native and container-intelligence management system that is included as part of your IBM Cloud architecture.
* [IBM Cloud Logs](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-cloud-logs-overview): This is a scalable logging service which is designed to persist logs while providing users with robust capabilities for querying, tailing, and visualizing their logs efficiently.
