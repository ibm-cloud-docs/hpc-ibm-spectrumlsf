---

copyright:
  years: 2025
lastupdated: "2025-03-24"

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

## 24 March 2025
{: #release-notes-mar2425}
{: release-note}

Bug Fix
:   To support the removal of ICN feature, created a new custom image for the creation of PAC.

## 07 March 2025
{: #release-notes-mar0725}
{: release-note}

IBM Customer Number (ICN) is not supported.
:   The current solution no longer requires `ibm_customer_number`(ICN) for entitlement check before deploying the solution for non-production use. The solution is now available for use without ICN validation. Users can provision up to a maximum of 10 static worker nodes for evaluation or non-production use cases. If the number of worker nodes exceeds 10, it becomes the user responsibility to obtain the necessary entitlement check and licensing for those additional nodes in the production environment. For production use or for evaluating greater than 10 worker nodes, the user must purchase the necessary LSF licenses. To purchase the license, go to [Purchasing licenses](https://www.ibm.com/docs/en/devops-test-embedded/9.0.0?topic=licenses-purchasing).

## 05 March 2025
{: #release-notes-mar0525}
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
