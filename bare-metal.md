---

copyright:
  years: 2026
lastupdated: "2026-06-18"

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

# Bare metal support
{: #bm-overview}

{{site.data.keyword.cloud_notm}} Bare Metal servers for VPC offer dedicated physical servers within the VPC environment, enabling high performance, low latency, and full control over the underlying hardware resources. These servers combine the isolation and control of dedicated infrastructure with VPC networking features such as private networking, security groups, and scalable, cloud-native connectivity.

A new feature has been introduced to allow users to provision Bare Metal servers for **static compute nodes**.

## Enabling bare metal servers
{: #enable-bm}

To provision static compute nodes as bare metal servers, set `enable_baremetal` to true and specify a supported bare metal profile in the `static_compute_instances` configuration.

Example configuration:

```text
static_compute_instances = [{
profile = "mx3d-metal-64x512"
count   = 1
image   = "hpc-lsf-fp15-compute-rhel810-v4"
}]
```
{: codeblock}

enable_baremetal = true

## Key considerations
{: #key-features}

Following are the key features of bare metal server support for static compute instances:

* Bare metal servers are supported only for static compute nodes.
* When `enable_baremetal` is set to true, `enable_dedicated_host` must be set to false. These options are mutually exclusive.
* LSF Pay-As-You-Go images are not supported on bare metal servers. Only custom images can be used.
* Bare metal servers do not support SDP boot volume expansion.
* Mixing virtual server instances (VSIs) and bare metal servers within `static_compute_instances` is not supported.

## Benefits
{: #benefits}

Following are the benefits of bare metal server support for static compute instances:

* Bare metal compute nodes in an IBM Spectrum LSF cluster provide dedicated hardware for batch processing workloads.
* They ensure consistent and predictable performance by removing virtualization overhead.
* They offer exclusive access to CPU, memory, and network resources.
* They reduce resource contention, ensuring stable, and consistent performance.
* They support reliable job execution for demanding and high-performance workloads.

## Deployment variables
{: #deployment-values}

The `enable_baremetal` variable is set to `true` to enable bare metal servers for the static compute nodes in the cluster. The default value is `false`. For more information on the variable, see [Deployment values](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-deployment-values).
