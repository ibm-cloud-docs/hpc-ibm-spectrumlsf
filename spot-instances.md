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
{:table: .aria-labeledby="caption"}

# Spot Instances
{: #si-overview}

Spot instances offer a cost-efficient option for running transient and fault-tolerant workloads on {{site.data.keyword.cloud_notm}}. They utilize unused cloud capacity and are available at significantly lower prices compared to standard virtual server instances.

Spot instances use surplus capacity. {{site.data.keyword.cloud_notm}} may reclaim Spot instances when capacity is needed elsewhere, making them suitable for dynamic workloads that can handle interruptions and be automatically recreated.

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

IBM Spectrum LSF supports Spot instances only for dynamic compute nodes provisioned through the LSF Resource Connector. If Spot instances are reclaimed by {{site.data.keyword.cloud_notm}}, the Resource Connector automatically provisions replacement nodes to maintain workload demand and cluster capacity.

Management nodes and static worker nodes are not supported with Spot instances, as they require consistent and predictable availability.

### Key Considerations
{: #si-key}

Before enabling Spot instances, review the following considerations:

* Spot instances are supported only for dynamic compute nodes.
* Spot instances are supported with Flex and GPU instance profiles that support Spot provisioning.
* Spot instances are not supported for management nodes, static worker nodes, or login nodes.
* Spot instances cannot be used with dedicated hosts or dedicated host groups.
* Spot capacity cannot be reserved or guaranteed.
* {{site.data.keyword.cloud_notm}} can reclaim Spot instances at any time when capacity is required.
* Applications running on Spot instances must be designed to handle interruptions and instance replacement.

## Enabling Spot Instances
{: #si-enable}

To enable Spot instances for dynamic compute nodes, configure the `dynamic_compute_instances` parameter and set `enable_spot_instances` to true.

You must also specify a Spot-supported Flex or GPU profile in the `profile` parameter.

**Example configuration:**

```text
dynamic_compute_instances = [{
    profile               = "bxf-2x8"
    count                 = 500
    image                 = "hpc-lsf-fp15-compute-rhel810-v3"
    enable_spot_instances = true
}]
```
{: codeblock}

## What happens to the data on a Spot instance when it is reclaimed by {{site.data.keyword.cloud_notm}}?
{: #data-si}

The behavior of data on a Spot instance depends on the configured **preemption policy**, which determines how {{site.data.keyword.cloud_notm}} handles the instance after a reclamation event.

Following are the list of available Preemption Policies:

### **Stop**
{: #stop}

When the preemption policy is set to **stop**:

* The instance is powered off when it is reclaimed.
* The boot volume and instance data are preserved.
* Storage charges continue to apply while the instance remains stopped.
* The instance can be restarted later if Spot capacity becomes available.

This option is suitable when the data stored on the VSI boot volume must be retained after a reclamation event.

### **Delete**
{: #delete}

When the preemption policy is set to **delete**:

* The VSI and its associated boot volume are permanently removed when reclaimed.
* Any data stored only on the VSI boot volume is lost.
* A replacement compute node is automatically provisioned when additional cluster capacity is required.

### **Default configuration**
{: #si-default}

By default, the cluster uses the delete preemption policy.

This configuration is recommended because {{site.data.keyword.spectrum_full_notm}} stores application data, and job-related files on a shared file system that is accessible from all cluster nodes. As a result, compute nodes are treated as disposable resources, and retaining reclaimed instances is typically unnecessary. Using the delete policy also helps avoid ongoing storage charges for stopped instances.

### **Changing the Preemption Policy**
{: #si-changing-pp}

To change the preemption policy from delete to stop, log in to a management node and update the following configuration file:

```text
/opt/ibm/lsf/conf/resource_connector/ibmcloudgen2/conf/ibmcloudgen2_templates.json
```
{: codeblock}

In the `extensions` section, set the `preemption` value to `stop`:

```text
"extensions": {
  "availability": {
    "class": "spot"
  },
  "availability_policy": {
    "host_failure": "restart",
    "preemption": "stop"
  }
}
```
{: codeblock}

After updating the configuration, newly provisioned Spot instances will use the specified preemption policy.

## What happens to a job if the {{site.data.keyword.cloud_notm}} reclaims the server while it is still running?
{: #job-reclaim}

Spot instances can be reclaimed by {{site.data.keyword.cloud_notm}} when the underlying compute capacity is required for other workloads. 

During a reclamation event:

1. {{site.data.keyword.cloud_notm}} sends a reclamation notification and the OS receives a shutdown signal.
2. A **systemd** service automatically invokes the shutdown script located at `/usr/local/bin/ibm-cloud-shutdown-script.sh` on the spot instance.
3. The shutdown script prevents the node from accepting new workload execution requests while allowing currently running jobs to continue execution during the shutdown grace period.
4. After the script runs, the node status is updated to `closed_Adm`, preventing any additional jobs from being scheduled on the node.
5. If the Spot instance is reclaimed before a running job completes, the job is interrupted and fails unless the workload or application provides its own checkpointing, restart, or recovery mechanism.
6. To maintain cluster capacity, the Resource Connector can automatically provision replacement compute nodes and make them available for future workload execution.

The following example shows a compute node in the `closed_Adm` state after a reclamation event:

```text
[lsfadmin@test-spot-10-241-0-12 ~]$ bhosts -w
HOST_NAME                     STATUS             JL/U    MAX    NJOBS    RUN  SSUSP  USUSP    RSV
test-spot-10-241-0-12       closed_Adm             -      1        0      0      0      0      0
test-spot-comp-1-7a82-001       ok                 -      2        0      0      0      0      0
test-spot-mgmt-1-7a82-001   closed_Full            -      0        0      0      0      0      0
test-spot-mgmt-1-7a82-002   closed_Full            -      0        0      0      0      0      0
[lsfadmin@test-spot-10-241-0-12 ~]$
```
{: codeblock}
