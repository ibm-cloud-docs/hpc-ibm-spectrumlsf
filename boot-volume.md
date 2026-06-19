---

copyright:
  years: 2026
lastupdated: "2026-06-19"

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

## Supported boot volume profiles
{: #suppoted-bv}

The supported boot volume profiles depend on the node type being deployed.

| Node type | Supported profiles |
| ----- | ----------- |
| Login instance | general-purpose, sdp |
| Management instances | general-purpose, sdp |
| Static compute instances | general-purpose, sdp |
| Dynamic compute instances | general-purpose, sdp, 5iops-tier, 10iops-tier, custom |
{: caption="Supported boot volume profiles" caption-side="bottom"}

## Boot volume attributes
{: #attributes-bv}

The `boot_volume` block supports the following attributes:

| Attribute | Description |
| ----- | ----------- |
| Profile | Boot volume storage profile. Supported values depend on the node type.|
| Size | Boot volume capacity in GB.|
| IOPS | Provisioned IOPS for supported profiles. |
| Bandwidth | Provisioned bandwidth in Mbps for supported profiles. |
{: caption="Boot volume attributes" caption-side="bottom"}

**Example**

```text
boot_volume = { 
  profile = "sdp" 
  size  = 200
  iops  = 3500
  bandwidth = 2500
}
```
{: codeblock}

## Boot volume profiles
{: #boot-volume-profiles}

A boot volume profile defines the performance characteristics of the boot disk. The different types of profiles are mentioned below.

### General Purpose profile
{: #gp-profile}

The general-purpose profile is the default IBM Cloud boot volume profile and provides balanced performance for most workloads.

#### Supported node types
{: #supported-types}

Following are the supported node types are:

* Login instances
* Management instances
* Static compute instances
* Dynamic compute instances

#### Configuration rules
{: config-rules}

The following table shows the configuration rules:

| Attribute | Value |
| ----- | ----------- |
| Size | 100 GB – 250 GB |
| IOPS | Must be null |
| Bandwidth | Must be null |
{: caption="General Purpose - Configuration rules" caption-side="bottom"}

IOPS and bandwidth are automatically determined by IBM Cloud based on the selected volume size. User-defined IOPS and bandwidth are not supported.
{: note}

**Example**

```text
login_instance = [{ 
  profile = "cx2d-4x8"
  image = "hpc-lsf-fp15-compute-rhel810-v4"

boot_volume = {
  profile = "general-purpose"  
  size	= 100
  iops	= null 
  bandwidth = null
}
}]
```
{: codeblock}

### SDP profile
{: #sdp-profile}

SSD Defined Performance (SDP) provides configurable performance and throughput for workloads requiring predictable storage performance.

#### Supported node types
{: #supported-sdp-type}

Following are the supported node types are:

* Login instances
* Management instances
* Static compute instances
* Dynamic compute instances

#### Configuration rules
{: config-rules-sdp}

The following table shows the configuration rules:

| Attribute | Value |
| ----- | ----------- |
| Size | 100 GB – 32000 GB |
| IOPS | 3,000 – 64,000 (or null) |
| Bandwidth | 1,000 – 8,192 Mbps (or null) |
{: caption="SDP - Configuration rules" caption-side="bottom"}

If IOPS or bandwidth are not specified, IBM Cloud automatically assigns values based on the volume size.
{: note}

**Example**

```text
management_instances = [{ 
  profile = "bx2-16x64" 
  count = 2
  image = "hpc-lsf-fp15-rhel810-v4"

boot_volume = { 
  profile = "sdp" 
  size	= 200
  iops	= 3500
  bandwidth = 2500
}
}]
```
{: codeblock}

## 5-IOPS tier profile
{: #3tier-profile}

The 5iops-tier profile is available only for dynamic compute instances and provides a predefined IOPS-per-GB performance tier.

#### Supported node types
{: #supported-3tier-type}

For 5 IOPS tier profile the supported node types is Dynamic compute instances only.

#### Configuration rules
{: config-rules-3tier}

The following table shows the configuration rules:

| Attribute | Value |
| ----- | ----------- |
| Size | 100 GB – 250 GB |
| IOPS | Must be null |
| Bandwidth | Must be null |
{: caption="5 IOPS Tier Profile - Configuration rules" caption-side="bottom"}

IOPS are automatically determined by the selected tier and volume size. Custom IOPS and bandwidth values are not supported.
{: note}

**Example**

```text
dynamic_compute_instances = [{ 
  profile = "bx2-4x16"
  count = 500
  image = "hpc-lsf-fp15-compute-rhel810-v4"

boot_volume = {
  profile = "5iops-tier" 
  size	= 200
  iops	= null 
  bandwidth = null
}
}]
```
{: codeblock}

## 10-IOPS tier profile
{: #10tier-profile}

The 10iops-tier profile provides a higher predefined IOPS-per-GB performance tier for dynamic compute nodes.

#### Supported node types
{: #supported-10tier-type}

For 10 IOPS tier profile the supported node types is Dynamic compute instances only.

#### Configuration rules
{: config-rules-10tier}

The following table shows the configuration rules:

| Attribute | Value |
| ----- | ----------- |
| Size | 100 GB – 250 GB |
| IOPS | Must be null  |
| Bandwidth | Must be null |
{: caption="10 IOPS Tier Profile - Configuration rules" caption-side="bottom"}

IOPS are automatically calculated by IBM Cloud based on the tier and selected size. Custom IOPS and bandwidth values are not supported.
{: note}

**Example**

```text
dynamic_compute_instances = [{ 
  profile = "bx2-4x16"
  count = 500
  image = "hpc-lsf-fp15-compute-rhel810-v4"
 
boot_volume = {
  profile = "10iops-tier" 
  size	= 200
  iops	= null 
  bandwidth = null
}
}]
```
{: codeblock}

### Custom profile
{: #custom-profile}

The custom profile allows explicit IOPS configuration for dynamic compute nodes.

#### Supported node types
{: #supported-custom-type}

For Custom profile the supported node types is Dynamic compute instances only.

#### Configuration rules
{: #config-rules-custom}

The following table shows the configuration rules:

| Attribute | Value |
| ----- | ----------- |
| Size | 100 GB – 250 GB |
| IOPS | 100 – 48,000 |
| Bandwidth | Must be null |
{: caption="Custom profile - Configuration rules" caption-side="bottom"}

**Notes:**
•	Custom IOPS values can be configured to meet specific workload requirements.
•	Bandwidth configuration is not supported.
•	This profile is available only for dynamic compute nodes.

**Example**

```text
dynamic_compute_instances = [{ profile = "bx2-4x16"
count = 500
image = "hpc-lsf-fp15-compute-rhel810-v4"

boot_volume = { profile = "custom" size	= 250
iops	= 3600 bandwidth = null
}
}]
```
{: codeblock}

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

* Boot volume size can be increased only; reducing the size of an existing volume is not supported.

2. **General-Purpose profile limitations**

* Maximum boot volume size is 250 GB.
* Boot volume size can only be increased; shrinking an existing volume is not supported.
* User-defined IOPS are not supported.
* User-defined bandwidth is not supported.

3. **Dynamic Compute profile limitations**

* 5iops-tier, 10iops-tier, and custom profiles are supported only for dynamic compute instances.
* Bandwidth configuration is supported only with the SDP profile.
* Custom profile requires user-defined IOPS and does not support bandwidth configuration.
