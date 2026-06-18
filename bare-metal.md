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

{{site.data.keyword.cloud_notm}} Bare Metal Servers for VPC offer dedicated physical servers within the VPC environment, enabling high performance, low latency, and full control over the underlying hardware resources. These servers combine the isolation and control of dedicated infrastructure with VPC networking features such as private networking, security groups, and scalable, cloud-native connectivity.

A new feature has been introduced to allow users to provision Bare Metal servers as static compute nodes.

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

If a bare metal profile is not provided, validation fails.
{: note}

## Limitations
{: #limitations}

The following constraints must be considered when bare metal support is enabled:

1. Image support

    * LSF Pay-As-You-Go (PAYGo) images are not supported on bare metal servers.
    * Only custom images can be used for bare metal static compute nodes.

2. Dedicated Host compatibility

    * Bare Metal servers cannot be used together with the dedicated host feature.

    * When `enable_baremetal = true` then `enable_dedicated_host` must be set to false.

3. Static compute node types

A deployment cannot contain a mix of Bare Metal and Virtual Server Instances (VSIs) within static_compute_instances. All static compute nodes must be of the same type:

* All Bare Metal nodes, or
* All VSI nodes

Mixed configurations are not supported and will fail validation.
{: note}


