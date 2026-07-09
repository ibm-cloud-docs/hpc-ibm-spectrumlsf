---

copyright:
  years: 2026
lastupdated: "2026-07-09"

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

{{site.data.keyword.cloud_notm}} Bare Metal servers offers dedicated physical servers within the VPC environment, enabling high performance, low latency, and full control over the underlying hardware resources. These servers provide the isolation and control of dedicated infrastructure while leveraging VPC networking capabilities, including private networking, security groups, and scalability.

A new feature has been introduced to allow users to provision Bare Metal servers for **static compute nodes**.
{: note}

The Bare Metal server capacities are limited and support for only specific regions. You need to check the server capacities are available in that region.
{: important}

## Enabling bare metal servers
{: #enable-bm}

To provision worker nodes as bare metal servers, set `enable_baremetal` to true and specify a supported bare metal profile in the `static_compute_instances` configuration. For more information on bare metal server profiles, see [x86-64 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui).

For more information on the variable, see [Deployment values](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-deployment-values).

Example configuration:

```text
enable_baremetal = true

static_compute_instances = [
  {
    profile = "mx3d-metal-64x512"
    count   = 1
    image   = "hpc-lsf-fp15-compute-rhel810-v4"
  }
]
```
{: codeblock}

## Key considerations
{: #key-features}

Following are the key features of bare metal server support for static compute instances:

* Bare metal servers are supported only for static compute nodes.
* When `enable_baremetal` is set to true, `enable_dedicated_host` must be set to false. These options are mutually exclusive.
* When LSF Pay-As-You-Go is enabled and bare metal is set to `true`, the automation selects the default images (v4). However, users can also provide a customized image for bare metal.
* Bare metal servers do not support boot volume expansion.
* Mixing virtual server instances (VSIs) and bare metal servers within `static_compute_instances` is not supported.

## Benefits
{: #benefits}

Following are the benefits of bare metal server support for static compute instances:

* Bare metal compute nodes in an IBM Spectrum LSF cluster provide dedicated hardware for batch processing workloads.
* They ensure consistent and predictable performance by removing virtualization overhead.
* They offer exclusive access to CPU, memory, and network resources.
* They reduce resource contention, ensuring stable, and consistent performance.
* They support reliable job execution for demanding and high-performance workloads.
