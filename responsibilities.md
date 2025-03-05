---

copyright:
  years: 2025
lastupdated: "2025-03-03"

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
{:table: .aria-labeledby="caption"}
{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when you use {{site.data.keyword.spectrum_full_notm}}
{: #responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use the {{site.data.keyword.spectrum_full_notm}} deployable architecture.
{: shortdesc}

- For more information about the responsibilities for you and for {{site.data.keyword.IBM}} when you use a deployable architecture, see [Understanding your responsibilities when you use deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-responsibilities-deployable-architectures).
- For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the user and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for using {{site.data.keyword.cloud}} products](/docs/overview?topic=overview-shared-responsibilities).
- For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms).

## Overview of shared responsibilities
{: #overview-by-shared-resource}

{{site.data.keyword.spectrum_full_notm}} is a product that is deployed to user resources in the [{{site.data.keyword.cloud_notm}} shared responsibility model](/docs/overview?topic=overview-shared-responsibilities). Start by reviewing the following table of who is responsible for particular cloud resources for {{site.data.keyword.spectrum_full_notm}}. Next, view more granular tasks for shared responsibilities in the proceeding sections.

If you use other {{site.data.keyword.cloud_notm}} products such as {{site.data.keyword.cos_short}}, responsibilities that are marked as yours in the following table, such as disaster recovery for Data, might be {{site.data.keyword.IBM_notm}}'s or shared. Consult those products' documentation for your responsibilities.
{: note}

| Resource | [Incident and operations management](#incident-and-ops) | [Change management](#change-management) | [Security and regulation compliance](#security-compliance) | [Disaster recovery](#disaster-recovery) |
| - | - | - | - | - |
| [Data](#applications-data) | You | You | You | You |
| [Application Orchestration](#applications-data) | You | You | You | You |
| Observability | [Shared](#incident-and-ops) |IBM | [Shared](#security-compliance) | IBM |
| App networking | You | You | You | You |
| Cluster networking | [Shared](#incident-and-ops) | [Shared](#change-management) | [Shared](#security-compliance) | You |
| Cluster version | [Shared](#incident-and-ops) | [Shared](#change-management) | Not applicable | Not applicable |
| Management nodes | [Shared](#incident-and-ops) | [Shared](#change-management) | [Shared](#security-compliance) | You |
| Compute nodes | [Shared](#incident-and-ops) | [Shared](#change-management) | [Shared](#security-compliance) | You |
| Virtual storage | [Shared](#incident-and-ops) | [Shared](#change-management) | [Shared](#security-compliance) | You |
| Virtual network | [Shared](#incident-and-ops) | [Shared](#change-management) | [Shared](#security-compliance) | You |
{: caption="Responsibilities by resource" caption-side="bottom"}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use the {{site.data.keyword.spectrum_full_notm}} deployable architecture.

## Incident and operations management
{: #incident-and-ops}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Management nodes| * Deploy highly available dedicated management nodes in a secured, IBM-owned infrastructure account for each cluster.  \n * Ensure the health of management nodes in OS level. | Use the provided console tools to request that management nodes are rebooted or reloaded, and troubleshoot issues such as when the management nodes are in an unhealthy state. |
|Compute nodes | * Provision compute nodes in VPC under your IBM Cloud infrastructure account.  \n * Ensure that compute nodes successfully provision when the user account and permissions are correctly set up, and a sufficient quota exists.  \n * Fulfill requests for more infrastructure, such as adding, reloading, updating, and removing compute nodes.  \n * Provide tools, such as the LSF Resource Connector to extend your cluster infrastructure.  \n * Fulfill automation requests to help recover compute nodes.  \n * Ensure the health of compute nodes in OS level. | * Use the provided API, CLI, or console tools to adjust storage capacity to meet the needs of your workload.  \n * Deploy application/tools in cluster |
|Cluster networking| * Set up cluster management components, such as public or private cloud service endpoints.  \n * Fulfill requests for more infrastructure, such as attaching worker nodes to existing VPC or subnets upon resizing a compute pool.  \n * Provide the ability to set up a VPN connection with on-premises resources such as through the strongSwan IPSec VPN service or the IBM Cloud VPC VPN.  \n * Provide the ability to isolate network traffic with login nodes. | Use IBM Cloud VPC tools to adjust networking configuration to meet the needs of your workload. |
|Observability| * Provide standard {{site.data.keyword.spectrum_full_notm}} tools for monitoring the status of LSF cluster.  \n * Provide a standard IBM Cloud Console for monitoring the status of VPC resources(VSI, network, storage, and so on). | Set up and monitor the health of your cluster health metrics. |
{: caption="Responsibilities for incident and operations" caption-side="bottom"}

## Change management
{: #change-management}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

You and IBM share responsibilities for keeping your clusters at the supported platform and operating system versions, along with recovering infrastructure resources that might require changes. You are responsible for change management of your application data.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Management nodes| Provide management node patch operating system(OS), version, and security updates for image used for new cluster creation. | Use the IBM Cloud tools to apply the provided(existing) management nodes updates that include operating system; or to request that management nodes are rebooted. |
|Compute nodes| Provide compute node patch operating system (OS), version, and security updates. Not supported on existing running VSIs, only for new VSIs with latest image. | Use IBM Cloud tools to apply the provided compute node updates that include operating system patches; or to raise ticket to request that worker nodes are rebooted. |
|Cluster version| Provide image for new version of LSF for new cluster creation. | Update existing management nodes and compute nodes to new LSF version, or create new cluster with latest image to run with new cluster version |
{: caption="Responsibilities for change management" caption-side="bottom"}

## Identity and access management
{: #iam-responsibilities}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

You and IBM share responsibilities for controlling access to your {{site.data.keyword.spectrum_full_notm}} instances. For IBM Cloud® Identity and Access Management responsibilities, consult that product's documentation. You are responsible for identity and access management to your application data.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Observability| Provide the ability to integrate IBM Cloud Activity Tracker with your cluster to audit the actions that users take in the cluster. | Set up IBM Cloud Activity Tracker or other capabilities to track user activity in the cluster. |
{: caption="Responsibilities for identity and access management" caption-side="bottom"}

## Security and regulation compliance
{: #security-compliance}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

IBM is responsible for the security and compliance of HPC Clusters on IBM Cloud. Compliance with industry standards varies depending on the infrastructure provider that you use for the cluster. You are responsible for the security and compliance of any workloads that run in the cluster and your application data.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|General| Provide security controls commensurate to best practice for {{site.data.keyword.spectrum_full_notm}} in Cloud.|
Provide options for cluster network connectivity, such as public and private cloud service endpoints | Set up and maintain security and regulation compliance for your apps and data. For example, choose how to set up your cluster network, protect sensitive information such as with IBM Key Protect encryption, and configure further security settings to meet your workload's security and compliance needs. If applicable, configure your firewall. |
|Management nodes|  | As part of your incident and operations management responsibilities for the management nodes, apply the provided security patch updates. |
|Compute nodes| Disable certain insecure actions for compute nodes, such as not permitting users to SSH into the host. | As part of your incident and operations management responsibilities for the worker nodes, apply the provided security patch updates. |
{: caption="Responsibilities for security and regulation compliance" caption-side="bottom"}

## Disaster recovery
{: #disaster-recovery}

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provisioning disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

IBM is responsible for the recovery of Spectrum Computing on IBM Cloud components if there is disaster. You are responsible for the recovery of the workloads that run the cluster and your application data. If you integrate with other IBM Cloud services such as file, block, object, cloud database, logging, or audit event services, consult those services' disaster recovery information.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|General|  | * Set up and maintain disaster recovery capabilities for your apps and data. For example, to prepare your cluster for HA/DR scenarios, follow the guidance in High availability on IBM Cloud. Note that persistent storage of data such as application logs and cluster metrics are not set up by default.  \n * Creating resources in a secondary region and managing the application and data disaster recovery. |
{: caption="Responsibilities for disaster recovery" caption-side="bottom"}

## Applications and data
{: #applications-data}

You are responsible for the applications, workloads, and data that you deploy to {{site.data.keyword.cloud_notm}}. However, {{site.data.keyword.IBM_notm}} provides various tools to help you set up, manage, secure, integrate, and optimize your applications.

| Resource | How {{site.data.keyword.IBM_notm}} helps | What you can do |
|----------|-----------------------|--------|
| Data | * Maintain platform-level standards so that your data can be stored with controls commensurate (refer to IBM File storage statement) to a minimum set of security compliance standards.  \n * Integrate with IBM Cloud services that you can use to store and manage your data, such as File Storage and Block Storage. | * Maintain responsibility for your data and how your apps consume the data.|
| Applications | * Provision clusters with Spectrum LSF, Data Manager, and License Scheduler.  \n * Generate an API key that is used to access infrastructure permissions for each resource group and region | * Maintain responsibility for your apps, data, and their complete lifecycle.  \n * Use the provided tools and features to configure and deploy; keep up to date; set up resource requests and limits; size your compute pool to have enough resources to run your apps; set up permissions; integrate with other services; externally serve; save, back up, and restore data; and otherwise manage your highly available and resilient workloads. |
{: caption="Responsibilities for applications and data" caption-side="bottom"}
