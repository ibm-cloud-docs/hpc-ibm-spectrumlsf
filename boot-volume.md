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

# Boot Volume Configuration
{: #boot-volume-overview}

Boot volumes are automatically created and attached during instance provisioning. The solution provides flexible boot volume configuration options to meet different workload requirements across login, management, static compute, and dynamic compute nodes.
If the `boot_volume` block is not specified, the deployment uses the default boot volume configuration.

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

## Resizing the Disk
{: #resize-disk}

For SDP profiles, you can increase the **disk size**, **IOPS**, and **bandwidth** values. Resizing can be performed either manually or through automation.

### Scenario 1: Manual Resize

To increase the disk size, IOPS, or bandwidth manually:

1. Update the required values for the disk. 
2. Updating the values does not automatically resize the disk. Perform the manual steps.
3. Verify that the updated values are reflected in the cluster and user interface.

### Scenario 2: Automation/Terraform

To increase the disk size, IOPS, or bandwidth using automation:

1. Update the required values in your Terraform configuration or automation workflow.
2. Reapply the configuration.
3. The updated values are automatically reflected in both the cluster and the user interface.

## Conclusion

- Disk size, IOPS, and bandwidth can only be increased. Decreasing these values is not supported.
- For nodes configured as **`general-purpose`**, **IOPS** and **bandwidth** must remain **null**. Specifying values for either parameter will result in an error during automation.
