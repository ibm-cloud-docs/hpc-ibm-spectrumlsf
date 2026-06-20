---

copyright:
  years: 2026
lastupdated: "2026-06-20"

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

Dedicated hosts allow you to deploy virtual server instances on single-tenant compute hosts, ensuring isolation from other users. This setup helps prevent noisy neighbor issues such as performance interference caused by shared workloads in public virtual server instances. When using a dedicated host, billing is based on host usage rather than individual vCPU or RAM consumption.

## Key Considerations
{: #key-considerations}

Following are the key factors to deploy the dedicated host:

* The solution supports deployment of both static and dynamic compute nodes exclusively on dedicated hosts. 

* There is no separate parameter to specify the dedicated host profile. The dedicated host family is automatically derived from the `static_compute_instances` parameter. For example, if the compute nodes use a bx2 profile, the dedicated hosts will be provisioned from the bx2 family.

* The current solution supports a single instance profile type from any of the supported families: bx2, cx2, mx2, third-generation, and so on.

For more information, go to [Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-dh-profiles&interface=ui).

## Before you begin
{: #before-you-begin}

Before you begin, make sure to complete the steps from [Before you begin deploying](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-getting-started-tutorial) topic.

## Benefits
{: #benefits-dedicated-hosts}

Following are the benefits of a dedicated host:

* **Consistent and High Performance** – Ideal for long-running workloads with demanding performance needs.

* **Enhanced Workload Management** – Provides better control over virtual server instance placement and resource allocation.

* **High Security and Compliance** – Ensures physical isolation of workloads, making it suitable for compliance-driven industries with strict data isolation and residency requirements.

## Enabling a Dedicated Host
{: #enable-dedicated-hosts}

To enable a dedicated host, set the `enable_dedicated_host` parameter to true (default: false). Once enabled, all the static and dynamic worker nodes are automatically attached to the same dedicated host.

| Dedicated host variable | Description | Example value |
| ----- | ----------- | --------------- |
| `enable_dedicated_host` | Set this option to `true` to enable dedicated hosts for the VSIs provisioned as workload servers. The default value is `false`. When the dedicated hosts are enabled, a single VSI profile is used for both static and dynamic node provisioning. Multiple profiles are not supported, as dedicated hosts are on single-tenant servers. Spot instances are not supported with dedicated hosts. If you plan to deploy a static cluster using a third-generation profile, verify that the chosen region supports dedicated hosts, since not all regions offer support for third-generation profiles on dedicated hosts. For more information about dedicated host, go to [Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-dh-profiles&interface=ui).| true |
{: caption="Configuring dedicated host deployment values" caption-side="bottom"}

For more information about dedicated host, go to [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=ui).

## Limitations
{: #limitations-dedicated-host}

1. **Single Profile Requirement**

    * When `enable_dedicated_host` is set to true, you must specify only one profile in `static_compute_instances` parameter.

    * If more than one profile is provided, an error is displayed:

    **Error Example:**

    ```console
    Error: Invalid value for variable
    │
    │ on userinput.auto.tfvars line 21:
    │ 21: static_compute_instances = [{
    │ profile = "bx2-4x16"
    │ count   = 1
    │ image   = "hpc-lsf-fp15-compute-rhel810-v4"
    │ },
    | {
    │ profile = "cx2-2x4"
    │ count   = 2
    │ image   = "hpc-lsf-fp15-compute-rhel810-v4"
    | }]
    ├────────────────
    │     │ var.enable_dedicated_host is true
    │     │ var.static_compute_instances is list of object with 2 elements
    │ 
    │When dedicated hosts are enabled, static_compute_instances must contain only a single instance profile entry.   Multiple profile entries are not supported, even if the profiles belong to the same VSI family.
    │ 
    │ This was checked by the validation rule at variables.tf:324,3-13.
    ```
    {: codeblock}

2. **Supported Profiles** 
    
    If a single profile from bx2, cx2, mx2, cx2d, mx2d, or bx2d is specified, then the dedicated host is created from the same family and all the worker nodes are assigned to it.

3. **Third-Generation Profile Limitation**

    * Third-generation profiles like mx3d, cx3d, and bx3d are only available in specific regions (Dallas, Frankfurt, Toronto, Madrid).

    * Deploying an unsupported profile in a different region results in a failure during the planning or early deployment stage.

    **Error Example:**

    If a profile "bx3d" is provided on us-east, then the build fails at planning or early stage of deployment stating that this profile is not supported.

    ```console
    Error: Invalid index
    │
    │ on locals.tf line 316, in locals:
    │ 316: dh_profile = var.enable_dedicated_host ? local.dh_profiles[local.dh_profile_index] : null
    │ ├────────────────
    │ │ local.dh_profile_index is "Profile class bx3d for dedicated hosts does not exist in us-east.  Check available class with ibmcloud target -r us-east; ibmcloud is dedicated-host-profiles and retry with another static_compute_instances."
    │ │ local.dh_profiles is empty tuple
    │
    │ The given key does not identify an element in this collection value: a number is required.
    ```
    {: codeblock}

4. **Spot Instances and Dedicated hosts cannot be used together**

The solution does not support deployments where both `enable_spot_instances` and `enable_dedicated_host` are set to true. To use dedicated hosts, Spot Instances must be disabled, and vice versa.

    **Error Example:**

    ```console
    │ Error: Invalid value for variable
    │ 
    │ on userinput.auto.tfvars line 27:
    │ 27: dynamic_compute_instances = [
    │ {
    │ profile = "bxf-2x8"
    │ count   = 500
    │ image   = "hpc-lsf-fp15-compute-rhel810-v4"
    │ enable_spot_instances = true
    │ }
    │ ]
    │ ├────────────────
    │ │ var.dynamic_compute_instances is list of object with 1 element
    │ │ var.enable_dedicated_host is true
    │ 
    │ Spot instances are not supported with dedicated hosts. When enable_spot_instances is true, enable_dedicated_host must be false.
    │ 
    │ This was checked by the validation rule at variables.tf:392,3-13.

When a dedicated host is enabled, the resource connector templates are automatically updated to include the host ID, allowing dynamic nodes to join.
{: note}
