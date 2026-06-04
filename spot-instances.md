---

copyright:
  years: 2026
lastupdated: "2026-06-04"

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

# Spot Instances
{: #si-overview}

Spot instances offer a cost-efficient option for running transient and fault-tolerant workloads on IBM Cloud. They utilize unused cloud capacity and are available at significantly lower prices compared to standard virtual server instances.

Spot instances use surplus capacity. IBM Cloud may reclaim Spot instances when capacity is needed elsewhere, making them suitable for dynamic workloads that can handle interruptions and be automatically recreated.

## Benefits
{: #si-benefits}

Spot instances can help reduce infrastructure costs for workloads that can tolerate interruptions. Some of the typical use cases include:

* High-throughput computing (HTC)
* Batch processing workloads
* Simulation workloads
* AI and machine learning training jobs
* Rendering workloads
* Large-scale parallel processing jobs

These workloads can benefit from significant cost savings while maintaining scalability through automatic node provisioning.

## Spot Instances for Dynamic Compute Nodes
{: #si-dynamic-nodes}

IBM Spectrum LSF supports Spot instances only for dynamic compute nodes provisioned through the LSF Resource Connector. If Spot instances are reclaimed by IBM Cloud, the Resource Connector automatically provisions replacement nodes to maintain workload demand and cluster capacity.

Management nodes and static worker nodes are not supported with Spot instances, as they require consistent and predictable availability.

### Key Considerations
{: #si-key}

Before enabling Spot instances, review the following considerations:

* Spot instances are supported only for dynamic compute nodes.
* Spot instances are supported with Flex and GPU instance profiles that support Spot provisioning.
* Spot instances are not supported for management nodes, static worker nodes, or login nodes.
* Spot instances cannot be used with dedicated hosts or dedicated host groups.
* Spot capacity cannot be reserved or guaranteed.
* IBM Cloud can reclaim Spot instances at any time when capacity is required.
* Applications running on Spot instances must be designed to handle interruptions and instance replacement.

## Enabling Spot Instances
{: #si-enable}

To enable Spot instances for dynamic compute nodes, configure the `dynamic_compute_instances` parameter and set `enable_spot_instances` to true.

You must also specify a Spot-supported Flex or GPU profile in the `profile` parameter.

**Example Configuration:**

```text
dynamic_compute_instances = [{
    profile               = "bxf-2x8"
    count                 = 500
    image                 = "hpc-lsf-fp15-compute-rhel810-v3"
    enable_spot_instances = true
}]
```
{: codeblock}
