---

copyright:
  years: 2026
lastupdated: "2026-06-15"

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

# SSD Defined Performance (SDP)
{: #sdp-overview}

The SSD Defined Performance (SDP) is a second-generation IBM Cloud boot volume profile that provides enhanced flexibility in defining performance and capacity characteristics for boot volumes. By using the `sdp` profile, you can specify the boot volume capacity and configure the maximum throughput limit.

## Benefits
{: #sdp-benefits}

* You can configure the boot volume size in the range from 100 GB to 32,000 GB.
* You can specify the boot volume performance in the range from 3000 IOPS to 64,000 IOPS.
* You can specify the maximum throughput limit in the range from 125 MBps to 1024 MBps (1000-8192 Mbps).
* Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning.

The SDP provides high I/O throughput required for:

* Metadata operations
* Data input/output heavy workloads
* Large file movement
* Parallel job access patterns

## Limitations of using SDP as boot volume
{: #limitations}

* When you create an instance from a custom image, you can specify a boot volume capacity of 100 GB to 250 GB. If the boot volume exceeds 250 GB, the Virtual Server Instance (VSI) fails to boot successfully.
* Boot volume size can only be increased; reducing the size is not supported to maintain data safety and integrity.

## Boot Volume
{: #boot-volume}

Boot volumes are automatically created and attached during VSI provisioning. To simplify deployment and ensure consistent performance across `login_instance`, `management_instances`, `static_compute_instances`, and `dynamic_compute_instances` the boot volume uses the general-purpose profile by default. However, this behavior can be overridden during provisioning by specifying a `SDP` profile when required by the workload.

When specifying for a `general-purpose` profile, the `iops` and `bandwidth` are not supported, so the value should be **null** which are automatically picked by the platform based on the **size**.
{: important}

## Deployment variables
{: #sdp-variables}

To enable SDP on a LSF cluster, the following variables needs to be defined:

### **login_instance**
{: #login}

In this variable, you can specify the login node configuration, including the instance profile, image name, and any optional boot volume settings.

#### Boot Volume Configuration (Optional)
{: #login-boot}

The `boot_volume` block can be used to customize the boot volume attached to the login node.

| Attribute | Description |
| ----- | ----------- |
| `profile` | Storage profile for the boot volume. Supported values: `general-purpose` or `sdp`.|
| `size` | Boot volume size is in GB. Minimum size is 100 GB. For general-purpose, the maximum size is 250 GB. For sdp, the initial deployment size must be between 100 and 250 GB, and can later be expanded up to 32000 GB through subsequent Terraform apply operations.|
| `iops` | Optional only when profile = "sdp" and must be between 3000 and 64000 or null. Must be null for general-purpose.|
| `bandwidth` | Optional when profile = "sdp" and must be between 1000 and 8192 MBps or null. Must be null for general-purpose. |
{: caption="login_instance - Boot Volume Configuration (Optional)" caption-side="bottom"}

**Example: Using default General-Purpose Boot Volume**

```text
login_instance = [
  {
    profile = "bx2-2x8"
    image   = "hpc-lsf-fp15-compute-rhel810-v3"

    boot_volume = {
      profile   = "general-purpose"
      size      = 150
      iops      = null
      bandwidth = null
    }
  }
]
```
{: codeblock}

**Example: Using SDP Boot Volume**

```text
login_instance = [
  {
    profile = "bx2-2x8"
    image   = "hpc-lsf-fp15-compute-rhel810-v3"

    boot_volume = {
      profile   = "sdp"
      size      = 200
      iops      = 3000
      bandwidth = 1000
    }
  }
]
```
{: codeblock}

### **management_instances**
{: #mgmt}

In this variable, you can specify the list of management node configurations, including instance profile, image name, instance count, and optional boot volume settings.

#### Boot Volume Configuration (Optional)
{: #mgmt-boot}

The `boot_volume` block can be used to customize the boot volume attached to management nodes.

| Attribute | Description |
| ----- | ----------- |
| `profile` | Storage profile for the boot volume. Supported values: `general-purpose` or `sdp`.|
| `size` | Boot volume size in GB. Minimum size is 100 GB. For general-purpose, the maximum size is 250GB. For sdp, the initial deployment size must be between 100 and 250 GB, and can later be expanded up to 32000 GB through subsequent Terraform apply operations.|
| `iops` | Optional only when profile = "sdp" and must be between 3000 and 64000 or null. Must be null for general-purpose.|
| `bandwidth` | Optional only when profile = "sdp". Must be between 1000 and 8192 Mbps or null. Must be null for general-purpose. |
{: caption="management_instances - Boot Volume Configuration (Optional)" caption-side="bottom"}

**Example: Using SDP Boot Volume**

```text
management_instances = [
  {
    profile = "bx2-16x64"
    count   = 2
    image   = "hpc-lsf-fp15-rhel810-v3"

    boot_volume = {
      profile   = "sdp"
      size      = 200
      iops      = 3000
      bandwidth = 1000
    }
  }
]
```
{: codeblock}

### **static_compute_instances**
{: #static}

Specify the list of static compute node configurations, including instance profile, image name, instance count, and optional boot volume settings.

#### Boot Volume Configuration (Optional)
{: #login-boot}

The boot_volume block can be used to customize the boot volume attached to static compute nodes.

| Attribute | Description |
| ----- | ----------- |
| `profile` | Storage profile for the boot volume. Supported values: general-purpose or sdp.|
| `size` | Boot volume size in GB. Minimum size is 100 GB. For general-purpose, the maximum size is 250 GB. For sdp, the initial deployment size must be between 100 and 250 GB, and can later be expanded up to 32000 GB through subsequent Terraform apply operations.|
| `iops` | Optional only when profile = "sdp" and must be between 3000 and 64000 or null. Must be null for general-purpose.|
| `bandwidth` | Optional only when profile = "sdp" and must be between 1000 and 8192 Mbps or null. Must be null for general-purpose. |
{: caption="static_compute_instances - Boot Volume Configuration (Optional)" caption-side="bottom"}

**Example: Using SDP Boot Volume**

```text
static_compute_instances = [
  {
    profile = "bx2-4x16"
    count   = 2
    image   = "hpc-lsf-fp15-compute-rhel810-v3"

    boot_volume = {
      profile   = "sdp"
      size      = 200
      iops      = 3000
      bandwidth = 1000
    }
  }
]
```
{: codeblock}

### **dynamic_compute_instances**
{: #dynamic}

Specify the list of dynamic compute node configurations, including instance profile, image name, maximum instance count, spot instance settings, and optional boot volume configuration.

#### Boot Volume Configuration (Optional)
{: #dynamic-boot}

The boot_volume block can be used to customize the boot volume attached to dynamic compute nodes.

| Attribute | Description |
| ----- | ----------- |
| `profile` | Storage profile for the boot volume. Supported values: general-purpose, sdp, 5iops-tier, 10iops-tier, and custom.|
| `size` | Boot volume size in GB. Must be between 100 and 250 GB.|
| `iops` | Optional for sdp and custom profiles. Must be null for general-purpose, 5iops-tier, and 10iops-tier.|
| `bandwidth` | Supported only for sdp. Must be null for all other profiles. |
{: caption="dynamic_compute_instances - Boot Volume Configuration (Optional)" caption-side="bottom"}

**Example: Using SDP Boot Volume**

```text
dynamic_compute_instances = [
  {
    profile               = "bx2-4x16"
    count                 = 1024
    image                 = "hpc-lsf-fp15-compute-rhel810-v3"
    enable_spot_instances = false

    boot_volume = {
      profile   = "sdp"
      size      = 200
      iops      = 3000
      bandwidth = 1000
    }
  }
]
```
{: codeblock}

**Example: Using Spot Instances with a Flex Profile**

```text
dynamic_compute_instances = [
  {
    profile               = "bx2d-4x16xf"
    count                 = 1024
    image                 = "hpc-lsf-fp15-compute-rhel810-v3"
    enable_spot_instances = true

    boot_volume = {
      profile   = "general-purpose"
      size      = 100
      iops      = null
      bandwidth = null
    }
  }
]
```
{: codeblock}

For more information on deployment variables, see [Deployment values](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-deployment-values).

### Steps to expand an attached boot volume manually
{: #boot-steps}

1. Identify the boot volume.
    The attached boot volume is at `/dev/vda` with the root file system on partition 3.

2. Expand the partition.
    * Run the following command on all the storage node VSIs to expand the data partition to match the resized block volume:

    ```pre
    growpart /dev/vda 3
    ```
    {: codeblock}

3. Resize the volume.
    * The size of the underlying disk has increased.
    * The OS partition and file system needs to be expanded on every storage node VSI.

4. Grow the file system.
    * Once the partitions are expanded, grow the file system. For XFS file systems, run:

    ```pre
    xfs_growfs /
    ```
    {: codeblock}

    or use the appropriate mount point for the block volume (for example, /gpfs/fs1).

### Verifying the file system
{: #verify-fs}

1. Ensure you have expanded both partitions and the file system using the appropriate commands (`growpart` and `xfs_growfs` or equivalent).

2. On each storage node VSI, run the following command:

    ```pre
    lsblk
    ```
    {: codeblock}

    This command confirms the updated device sizes, partition growth, and correct mount points.

## Updating boot volume using UI
{: #create-ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your credentials.
2. Go to the **Navigation Menu**.
3. Click **Infrastructure** > **Compute** > **Virtual server instances**.
4. Click the virtual server instances created during cluster provisioning.
5. Click **Storage**.
6. Under Storage volumes, click the required **Boot volume** or **Data volume**.
7. Edit the required **Size**.

## Updating boot volume using CLI
{: #update-cli}

1. Run the following command to increase the capacity of the filesystem:

    ```pre
    ibmcloud is volume-update <Volume-ID> --capacity <capacity-in-GB>
    ```
    {: codeblock}

    For example, `ibmcloud is volume-update r010-9796daf5-5f24- 4b12-9a0c-a25456917edf --capacity 180`

2. Run the following command to increase the IOPS when the profile is set as `sdp`.

    ```pre
    ibmcloud is volume-update <Volume-ID> --iops <iops-value>
    ```
    {: codeblock}

    For example, `ibmcloud is volume-update r010-9796daf5-5f24-4b12-9a0c-a25456917edf --iops 3001`

You can perform the steps manually or using CLI, but the recommended way is using automation.
{: tip}



## References
{: #references}

* [Block Storage for VPC profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui)
* [Bandwidth allocation for Block Storage volumes](/docs/vpc?topic=vpc-block-storage-bandwidth&interface=ui)
