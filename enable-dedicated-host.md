---

copyright:
  years: 2025
lastupdated: "2025-02-20"

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
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Dedicated Hosts for Virtual Server Instances
{: #dedicated-hosts-vsi}

Dedicated hosts allows you to deploy virtual server instances on single-tenant compute hosts, ensuring isolation from other users. This setup helps prevent noisy neighbor issues such as performance interference caused by shared workloads in public virtual server instances. When using a dedicated host, billing is based on host usage rather than individual vCPU or RAM consumption.

## Key Considerations
{: #key-considerations}

Following are the key factors to deploy the dedicated host:

* This offering supports only static compute nodes on dedicated hosts.

* The number and profile names of dedicated hosts are determined by the `worker_node_instance_type` parameter.

* The current solution supports a single instance profile type from any of the supported families: bx2, cx2, mx2, etc.

For more information, go to [Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-dh-profiles&interface=ui).

## Enabling Dedicated Host
{: #enable-dedicated-hosts}

To enable a dedicated host, set the `enable_dedicated_host` parameter to true (default: false). Once enabled, all the static worker nodes are automatically attached to the same dedicated host.

## Limitations
{: #limitations-dedicated-host}

1. Single Profile Requirement
  * When `enable_dedicated_host` is set to true, you must specify only one profile in `worker_node_instance_type` parameter.

  * If more than one profile is provided, an error is displayed:

  Error Example:

    ```console
    Error: Invalid value for variable
    │
    │ on terraform.tfvars line 82:
    │ 82: enable_dedicated_host = true
    │ ├────────────────
    │ │ var.enable_dedicated_host is true
    │ │ var.worker_node_instance_type is list of object with 2 elements
    │
    │ When 'enable_dedicated_host' is true, only one profile should be specified in 'worker_node_instance_type'.
    │
    │ This was checked by the validation rule at variables.tf:688,3-13.
    ```
    {: codeblock}

2. Supported Profiles
  If a single profile from bx2, cx2, mx2, cx2d, mx2d, or bx2d is specified, then the dedicated host is created from the same family and all the worker nodes are assigned to it.

3. Third-Generation Profile Limitation

* Third-generation profiles like mx3d, cx3d, and bx3d are only available in specific regions (Dallas, Frankfurt, Toronto, Madrid).

* Deploying an unsupported profile in a different region results in a failure during the planning or early deployment stage.
  Error Example:
   If a profile "bx3d" is provided on us-east, then the build fails at planning or early stage of deployment stating that this profile is not supported.

    ```console
    Error: Invalid index
    │
    │ on locals.tf line 316, in locals:
    │ 316: dh_profile = var.enable_dedicated_host ? local.dh_profiles[local.dh_profile_index] : null
    │ ├────────────────
    │ │ local.dh_profile_index is "Profile class bx3d for dedicated hosts does not exist in us-east.  Check available class with ibmcloud target -r us-east; ibmcloud is dedicated-host-profiles and retry with another worker_node_instance_type."
    │ │ local.dh_profiles is empty tuple
    │
    │ The given key does not identify an element in this collection value: a number is required.
    ```
    {: codeblock}

## Benefits
{: #benefits-dedicated-hosts}

Following are the benefits of dedicated host:

* Consistent and High Performance – Ideal for long-running workloads with demanding performance needs.

* Enhanced Workload Management – Provides better control over virtual server instance placement and resource allocation.

* High Security and Compliance – Ensures physical isolation of workloads, making it suitable for compliance-driven industries with strict data isolation and residency requirements.

## Before you begin
{: #before-you-begin}

Before you begin, make sure to complete the steps from [Before you begin deploying](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-getting-started-tutorial) topic.

## Configuring dedicated host deployment values
{: #config-dedicated-hosts-deploy-values}

Define the following variable to enable a dedicated host on an LSF cluster:

| Dedicated host variable | Description | Example value |
| ----- | ----------- | --------------- |
| `enable_dedicated_host` | Set this option to `true` to enable dedicated hosts for the VSI created for workload servers. The default value is false. When a dedicated host is enabled, the solution supports only static worker nodes with a single profile. Multiple profile combinations are not supported. For example, you can select a profile from a single family, such as bx2, cx2, or mx2. If you are provisioning a static cluster with a third-generation profile, ensure that the dedicated hosts are supported in the chosen regions, as not all regions support dedicated hosts for third-generation profiles. For more information about dedicated host, go to [Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-dh-profiles&interface=ui).| true |
{: caption="Configuring dedicated host deployment values" caption-side="bottom"}

For more information about dedicated host, go to [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=ui).
