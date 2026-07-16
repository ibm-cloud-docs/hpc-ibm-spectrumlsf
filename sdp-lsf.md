---

copyright:
  years: 2026
lastupdated: "2026-07-16"

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
{: #boot-volume-overview}

Boot volumes are automatically created and attached during instance provisioning. The solution provides flexible boot volume configuration options to meet different workload requirements across login, management, static compute, and dynamic compute nodes.
If the `boot_volume` block is not specified, the deployment uses the default boot volume configuration.

For more information, see [SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#defined-performance-profile).

## Supported profiles for LSF solution
{: #suppoted-bv}

The supported boot volume profiles depend on the node type being deployed.

| Node type | Supported profiles | Default profile |
| ----- | ----------- | -------- | 
| Login instance | general-purpose, sdp | general-purpose |
| Management instances | general-purpose, sdp | general-purpose |
| Static compute instances | general-purpose, sdp | general-purpose |
| Dynamic compute instances | general-purpose, sdp, 5iops-tier, 10iops-tier, custom | general-purpose |
{: caption="Supported boot volume profiles" caption-side="bottom"}

## Profile comparison
{: #profile-comparison}

The following table shows the profile comparison:

| Profile | Supported Nodes | Size Range | IOPS | Bandwidth |
| ----- | ----------- | --------- | --------- | ----------- |
| general-purpose | Login, Management, Static, Dynamic | 100–250 GB | Auto | Auto |
| sdp | Login, Management, Static, Dynamic  | 100–32000 GB | 3,000–64,000 | 1,000–8,192 Mbps |
| 5iops-tier | Dynamic only |100–250 GB | Auto (5 IOPS/GB tier) | Not supported |
| 10iops-tier | Dynamic only |100–250 GB | Auto (10 IOPS/GB tier) | Not supported |
| custom | Dynamic only |100–250 GB | 100–48,000 | Not supported |
{: caption="Profile comparison" caption-side="bottom"}

## Limitations
{: #limitations-sdp}

Following are the limitations of different profiles:

1. **SDP profile limitation** 

    * The boot volume size can only be increased. Decreasing the size of an existing boot volume is not supported.

2. **General-Purpose profile limitations**

    * Maximum boot volume size is 250 GB.
    * Boot volume size can only be increased; shrinking an existing volume is not supported.
    * User-defined IOPS are not supported.
    * User-defined bandwidth is not supported.

3. **Dynamic Compute profile limitations**

    * 5iops-tier, 10iops-tier, and custom profiles are supported only for dynamic compute instances.
    * Bandwidth configuration is supported only with the SDP profile.
    * Custom profile requires user-defined IOPS and does not support bandwidth configuration.

## Resizing the disk
{: #resize-disk}

For SDP profiles, you can increase the **disk size**, **IOPS**, and **bandwidth** values. Resizing can be performed either manually or through automation.

### Scenario 1: Manual resize
{: #scenario1}

To increase the disk size, IOPS, or bandwidth manually:

1. Update the required values for the disk. 
2. Updating the values does not automatically resize the disk. 

    Refer the [Steps to expand an attached boot volume manually](/docs-draft/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-boot-volume-overview#boot-steps) section to perform the manual steps.
3. Verify that the updated values are reflected in the cluster and user interface.

#### Steps to expand an attached boot volume manually
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

5. Ensure you have expanded both partitions and the file system using the appropriate commands (`growpart` and `xfs_growfs` or equivalent).

6. On each storage node VSI, run the following command:

    ```pre
    lsblk
    ```
    {: codeblock}

    This command confirms the updated device sizes, partition growth, and correct mount points.

### Scenario 2: Automation/Terraform
{: #scenario2}

To increase the disk size, IOPS, or bandwidth using automation:

1. Update the required values in your Terraform configuration or automation workflow.
2. Reapply the configuration.
3. The updated values are automatically reflected in both the cluster and the user interface.

## Conclusion
{: #conclusion}

- Disk size, IOPS, and bandwidth can only be increased. Decreasing these values is not supported.
- For nodes configured as **`general-purpose`**, **IOPS** and **bandwidth** must remain **null**. Specifying values for either parameter will result in an error during automation.
