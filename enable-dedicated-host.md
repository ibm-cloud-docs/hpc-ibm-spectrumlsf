---

copyright:
  years: 2025
lastupdated: "2025-02-14"

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

# Enabling dedicated hosts
{: #enable-dedicated-hosts}

Dedicated hosts enable you to deploy your virtual server instances on single-tenant compute hosts. Workloads under dedicated hosts can avoid noisy neighbor issues (such as, performance interference due to other users workloads) that they might encounter on public virtual server instances. When you use a dedicated host, you are billed by usage of the host, not the vCPUs, or RAM associated with your virtual instances.

This offering can deploy static compute nodes on dedicated hosts. The number of dedicated hosts and the profile names for dedicated hosts are calculated from `worker_node_min_count` and `worker_node_instance_type`.

Following are the key values for enabling a dedicated host:

* When a dedicated host is enabled, you cannot have multiple profiles.
  Dedicated host ensures no co-tenancy with other users offering enhanced privacy and security. It provides isolation at the hardware level, which is critical for sensitive workloads or regulated environments.

* When a dedicated host is enabled, the length of the `worker_node_instance_type` should be equal to 1. But if the length is more than 1, then it displays an error.

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

* When a single profile "bx2/mx2/cx2/cx2d/mx2d/bx2d" value is passed to the `worker_node_profile_type` parameter, then the dedicated host gets created from the same group and all worker nodes join the same dedicated host.

* When a third generation profile "mx3d,cx3d/bx3d" value that is only available on certain regions (Dallas,Frankurt,Toronto, and Madrid) are passed, then the build fails.

  For example, if a profile "bx3d" is provided on us-east, then the build fails at planning or early stage of deployment stating that this profile is not supported.

  ```console
  Error: Invalid index
  │
  │ on locals.tf line 316, in locals:
  │ 316: dh_profile = var.enable_dedicated_host ? local.dh_profiles[local.dh_profile_index] : null
  │ ├────────────────
  │ │ local.dh_profile_index is "Profile class bx3d for dedicated hosts does not exist in us-east.Check available class with ibmcloud target -r us-east; ibmcloud is dedicated-host-profiles and retry with another worker_node_instance_type."
  │ │ local.dh_profiles is empty tuple
  │
  │ The given key does not identify an element in this collection value: a number is required.
  ```
  {: codeblock}

* Dedicated hosts are ideal for workloads with consistent and high-performance requirements for long-running workloads.

* Dedicated hosts allows you to manage your VSIs that are placed within a host, offering better control over workload distribution.

* Dedicated host have high-security environments with physical isolation, compliance-driven industries with strict data isolation and residency requirements.

By setting the `enable_dedicated_host` value to true, a dedicated host is deployed and ensures that all the static worker nodes are created on the dedicated host.

## Before you begin
{: #before-you-begin}

Before you begin, make sure to complete the steps from [Before you begin deploying](/docs-draft/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-getting-started-tutorial&interface=ui) topic.

## Configuring dedicated host deployment values
{: #config-dedicated-hosts-deploy-values}

Define the following variable to enable a dedicated host on an LSF cluster:

| Dedicated host variable | Description | Example value |
| ----- | ----------- | --------------- |
| `enable_dedicated_host` | Set this option to `true` to enable dedicated hosts for VSI created for the workload servers, with the default value set to false. | true |
{: caption="Configuring dedicated host deployment values" caption-side="bottom"}

To learn more about dedicated host, go to [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=ui).
