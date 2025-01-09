---

copyright:
  years: 2025
lastupdated: "2025-01-09"

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

Dedicated hosts enable you to deploy your virtual server instances on single-tenant compute hosts. Workloads under dedicated hosts can avoid noisy neighbor issues (for example, performance interference due to other users workloads) that they might encounter on public virtual server instances. When you use a dedicated host, you are billed by the usage of the host, not vCPUs or RAM associated with your virtual instances.

This offering can deploy static compute nodes on dedicated hosts. The number of dedicated hosts and the profile names for dedicated hosts are calculated from `worker_node_min_count` and `worker_node_instance_type`.

Following are the key values for enabling dedicated host:

* Dedicated host ensures no co-tenancy with other customers offering enhanced privacy and security. It provides isolation at the hardware level, which is critical for sensitive workloads or regulated environments.

* Dedicated hosts are ideal for workloads with consistent and high-performance requirements for long-running workloads.

* It allows you to manage where your VSIs are placed within a host, offering better control over workload distribution.

* High-security environments requiring physical isolation.

* Compliance-driven industries with strict data isolation and residency requirements.

By setting the `enable_dedicated_host` value to true, a dedicated host will be deployed and ensures that all the static worker nodes will be created on the dedicated host.

## Before you begin
{: #before-you-begin}

Before you begin, make sure to complete the steps for [Before you begin deploying](/docs-draft/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-getting-started-tutorial&interface=ui).

## Configuring dedicated host deployment values
{: #config-dedicated-hosts-deploy-values}

To enable dedicated host on a LSF cluster, the following variable needs to be defined:

| Dedicated Host variable | Description | Example value |
| ----- | ----------- | --------------- |
| `enable_dedicatedhost` | Set this option to true to enable dedicated hosts for the VSI created for workload servers, with the default value set to false. | true |
{: caption="Configuring dedicated host deployment values" caption-side="bottom"}

To learn more about dedicated host, click [here](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=ui).
