---

copyright:
  years: 2025
lastupdated: "2025-06-30"

keywords: IBM Spectrum LSF release notes

subcollection: hpc-ibm-spectrumlsf

content-type: release-note

---



{{site.data.keyword.attribute-definition-list}}
{:external: target="_blank" .external}
{:release-note: data-hd-content-type='release-note'}



# Release notes for {{site.data.keyword.spectrum_full_notm}}
{: #my-service-relnotes}

The release notes describes the brief overview of the new features, enhancements, known and fixed issues added to {{site.data.keyword.spectrum_full}} for the release.
{: shortdesc}

## July 2025
{: #subcollection-june25}

### 02 July 2025
{: #subcollection-july0225}
{: release-note}

**For this release, the DA tile version is 3.1.0**
Following are the change updates made for this release:

Support for Fix Pack 15 (FP15)
:   For this release, by default **Fix Pack 15** is supported along with Fix Pack 14 (FP14). This fix pack contains all the security fixes, vulnerabilities fixes, resource connectors and so on.
User can still do the cluster deployment through FP14.

Support for Web service
:   IBM Spectrum LSF Web Services is introduced in Fix Pack 15, which provides a RESTful HTTP(s) interface for remotely interacting with LSF clusters. This allow users to programmatically submit, monitor, and manage jobs, enabling seamless integration of LSF workload management into custom applications, web portals, and automation workflows.

Support for Process manager
:   IBM Spectrum LSF Process Manager is enabled by default as part of the LSF suite deployment. This helps users to automate, monitor, and control application workflows and dependencies across distributed computing environments.

Support for deployment using stock image
:   For this release, along with the custom image, user can do the deployment using the **stock image** also. The stock images can be used only for management and compute nodes and not for the deployer and dynamic nodes.

Updated the DA `landing_zone` and `landing_zone_vsi` module version.
:   The DA module versions for the landing zones are updated.

Support for Security and Compliance Center (SCC) Workload Protection
:   For this release, previously used SCC instances are deprecated and the new version of **SCC Workload Protection** is supported. Cloud-Native Application Protection Platform solution to manage your security and compliance posture, allowing you to monitor misconfigurations and detect and respond to vulnerabilities and threats in real-time.

Support for Deployer node
:   For this release, the entire cluster deployment is handled by the **deployer** node.

End to end deployment done through Ansible playbooks
:   Previously, all the configurations for {{site.data.keyword.spectrum_short}} were done using the user data through shell script. Now the cluster deployments and configurations are managed and handled by **Ansible** playbooks.

Application centre option is enabled by default for FP14 and FP15
:   From the previous release, user had a choice to enable or disable the application centre feature as an option. For this release, **application centre** option is enabled by **default** for both FP14 and FP15.

## April 2025
{: #subcollection-apr25}

### 15 April 2025
{: #subcollection-apr1525}
{: release-note}

Following are the breaking change updates made for the release:
* All the VNI’s are placed under the same resource groups.
* Updated the DA `landing_zone` and `landing_zone_vsi` module version along with the IBM provider version.
* Fixed the custom image builder bug, to support custom image deployment through Mac and Linux based VSI.

## March 2025
{: #subcollection-mar25}

### 24 March 2025
{: #subcollection-mar2425}
{: release-note}

Bug Fix
:   To support the removal of ICN feature, created a new custom image for the creation of PAC.

### 07 March 2025
{: #subcollection-mar0725}
{: release-note}

IBM Customer Number (ICN) is not supported.
:   The current solution no longer requires `ibm_customer_number`(ICN) for entitlement check before deploying the solution for non-production use. The solution is now available for use without ICN validation. Users can provision up to a maximum of 10 static worker nodes for evaluation or non-production use cases. If the number of worker nodes exceeds 10, it becomes the user responsibility to obtain the necessary entitlement check and licensing for those additional nodes in the production environment. For production use or for evaluating greater than 10 worker nodes, the user must purchase the necessary LSF licenses. To purchase the license, go to [Purchasing licenses](https://www.ibm.com/docs/en/devops-test-embedded/9.0.0?topic=licenses-purchasing).

### 05 March 2025
{: #subcollection-mar0525}
{: release-note}

In this release, IBM Spectrum LSF deployable architecture is introduced. Spectrum LSF is a scheduling software to enable High-Performance Computing (HPC) clusters. This offering uses deployable architecture to provision and configure {{site.data.keyword.cloud_notm}} resources. For more information, refer [Overview of IBM Spectrum LSF](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-about-spectrum-lsf).

### What's New
{: #what-new}

The following new features are added as part of this release:

* [IBM Spectrum LSF deployable architecture](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-ibm-spectrum-lsf): Spectrum LSF enables High-Performance Computing (HPC) clusters by using LSF as the HPC scheduling software. This solution employs a deployable architecture to provision and configure IBM Cloud resources.
* [IBM Cloud Activity Tracker Event Routing](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-activity-tracker-overview): This is a platform service which manages the auditing events at the account-level by configuring targets and routes that define where auditing data is routed.
* [Multiple static worker node profile support](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-considerations-for-HPC-custer-compute-types): This solution supports the creation of static compute nodes by leveraging different instance type profiles based on resource requirements.
* [PAC High Availability (HA) support](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-before-deploy-application-center): This allows LSF Application Center to run on all deployed management nodes, use a cross availability zone instance of the IBM Cloud® Database for MySQL as the backend database, and use an IBM Cloud® Application Load Balancer for VPC (ALB) as the VPC load balancer.
* [IBM Storage Scale support](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-integrating-scale): By using Storage Scale as your storage solution, you first set up a Storage Scale cluster, and then integrate a list of values from the Storage Scale deployment with the IBM Spectrum LSF cluster deployment. This provides more performance and scalability than standard file storage solutions.
* [IBM Cloud Monitoring](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-cloud-monitoring-overview): This is a cloud-native and container-intelligence management system that is included as part of your IBM Cloud architecture.
* [IBM Cloud Logs](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-cloud-logs-overview): This is a scalable logging service which is designed to persist logs while providing users with robust capabilities for querying, tailing, and visualizing their logs efficiently.
