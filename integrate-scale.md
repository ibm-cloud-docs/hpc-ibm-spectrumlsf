---

copyright:
  years: 2025
lastupdated: "2025-08-04"

keywords:
subcollection: hpc-ibm-spectrumlsf

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}

# Integrating {{site.data.keyword.scale_full_notm}} with your {{site.data.keyword.spectrum_full_notm}} cluster
{: #integrating-scale}

After deploying your Storage Scale cluster with CES, set up your {{site.data.keyword.spectrum_full_notm}} cluster to utilize the CES NFS mount points as a shared file storage solution.
{: shortdesc}

## Deploying your {{site.data.keyword.scale_short}} cluster
{: #scale-tile}

Deploy your {{site.data.keyword.scale_short}} cluster by using the [{{site.data.keyword.scale_short}} catalog deployment tile](https://cloud.ibm.com/catalog/content/ibm-spectrum-scale-d722b6b6-8bb5-4506-8f0f-03a5f05a3d6e-global) by using the {{site.data.keyword.cloud_notm}} console UI. Refer to the [{{site.data.keyword.scale_short}} documentation](/docs/storage-scale?topic=storage-scale-creating-workspace&interface=ui) for detailed steps.

When you create this workspace during {{site.data.keyword.scale_short}} cluster deployment:
1. Select to use product version 2.7.0 or later.
2. [Configure CES deployment values](/docs/storage-scale?topic=storage-scale-config-ces-integration-ldap-authentication#beforeyoubegin-config-ces) for your {{site.data.keyword.scale_short}} cluster by enabling the CES feature:
* Update the `total_protocol_cluster_instances` deployment value to be greater than or equal to **2** for high availability.
* Configure the necessary NFS mount points by updating the `filesets` value. This configuration creates independent file sets that act as NFS mount points for your {{site.data.keyword.spectrum_full}} cluster.
* Once the Scale cluster is successfully created, login to the CES node to run the following command.

Retrieve the NFS mount point from Storage Scale. By default, two NFS exports are created: /gpfs/fs1/data and /gpfs/fs1/tools.

```pre
# mmnfs export list
Path                Delegations                 Clients
/gpfs/fs1/tools         NONE                  10.241.0.0/20
/gpfs/fs1/data          NONE                  10.241.0.0/20

# mmlscluster --ces
GPFS cluster information
GPFS cluster name:    test-scale-poc.strgscale.com
GPFS cluster id:      70671008535366959

Cluster Export Services global parameters
Shared root directory:         /gpfs/fs1
Enabled Services:              NFS
Log level:                      0
Address distribution policy:   even-coverage

Node            Daemon node name                    IP address      CES IP address list
  6        test-scale-poc-ces-001.strgscale.com     10.241.16.12          10.241.17.4
  7        test-scale-poc-ces-002.strgscale.com     10.241.16.13          10.241.17.5

# mmlsfileset fs1
Filesets in file system 'fs1':
Name          Status          Path                                    
root          Linked       /gpfs/fs1                               
data          Linked       /gpfs/fs1/data                          
tools         Linked       /gpfs/fs1/tools
```

## Integrating Scale NFS mount points on {{site.data.keyword.spectrum_full_notm}} cluster
{: #integrate-scale-and-nfs}

After you deploy and verify your {{site.data.keyword.scale_short}} cluster, you [deploy your {{site.data.keyword.spectrum_full_notm}} cluster](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-deploy-architecture&interface=ui).

* During the LSF cluster creation, use the Storage Scale VPC. Under the `vpc_name` parameter, provide the name of the VPC created through the scale cluster.

* To create the LSF management node and compute worker nodes use the compute subnets (comp-pvt-1) from the Storage Scale cluster. Under the `cluster_subnet_id` parameter, provide the compute subnet ID for the cluster.

* To create the bastion/deployer and login nodes on the LSF, you should create a new subnet under the Scale VPC cluster. There is already an existing `login_subnet_id` created for the Storage Scale cluster. Provide the subnet ID for the cluster under the `login_subnet_id` parameter. Using the `storage_subnet_id` is not advised due to security issues. This approach ensures that the bastion/deployer and login node do not have a direct access to the Storage Scale nodes, which aligns with the planned architecture.

    The new subnet created should have the Public Gateway (PGW) attached, and this is required for the deployer node to clone the terraform code for the deployment process. For more information on how to attach the PGW, see [Working with subnets in VPC](https://cloud.ibm.com/docs/vpc?topic=vpc-subnets-configure&interface=ui).
    {: note}

* Provide the existing custom resolver ID under the `dns_custom_resolver_id` parameter. Since a custom resolver ID was already created under the Scale VPC, failing to provide this details cause the LSF deployment to fail.

* Provide the Storage Scale cluster storage security group ID under the `storage_security_group_id` parameter. This security group ID is required to establish the connection from the LSF cluster nodes to Storage Scale CES nodes from where the NFS mount points are exported.

* To use the Storage Scale CES NFS mount points on the LSF cluster nodes, ensure to pass the mount point details under the `custom_file_shares` parameter.

When client subnets are created, you can still use them as the login subnet.
{: tip}

If you want choose or opt the existing bastion setup on LSF, then refer the documentation for [Bastion node](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-bastion-node-overview).

Example:

```text
default = [{ mount_path = "/mnt/vpcstorage/tools", size = 100, iops = 2000 }, { mount_path = "/mnt/scale/tools", nfs_share = "test-scale-poc-ces.cesscale.com:/gpfs/fs1/tools" }, { mount_path = "/mnt/scale/data", nfs_share = "test-scale-poc-ces.cesscale.com:/gpfs/fs1/data" }]
```
{: codeblock}

From the preceding example, the derivations are as follows:

* /mnt/vpcstorage/tools - this is used to create and share all the LSF binaries as the shared directory. It stores all the logs and configurations.
* /mnt/scale/tools - this is the NFS mount point that is created on the CES node.
* /mnt/scale/data - this is the NFS mount point that is created on the CES node.
* For sharing the `nfs_share`, the automation needs <cluster-prefix-ces.cesscale.com>.
    1. The `resource_prefix` value for {{site.data.keyword.scale_short}}, such as **LSF**.
    2. A hyphen (-).
    3. The text **ces**.
    4. A dot (.)

You can use 'n' number of exports that is created from the Scale cluster. Make sure to update the values and the mount path names appropriately.
{: note}

* When using the Scale NFS, it is expected that all the login, management, and worker nodes share the NFS mount points. From the preceding example, the management and the worker nodes get the NFS mounted as the shared file system.

However, from Step 3 when you create a new subnet for creating the login node, there are few configuration changes that need to be done to export a new NFS called `/gpfs/fs1/lsf`. The default mount points `/gpfs/fs1/data` and `/gpfs/fs1/tool` are created through the compute subnet CIDR range. When the login subnet is created through a different CIDR then the default mount points cannot be accessed as the ranges are different and from the login CIDR range a new export should be created. Run the following commands:

```text
# mmcrfileset fs1 new_fileset --inode-space new
# mmlinkfileset fs1 new_fileset -J /gpfs/fs1/new_fileset
```
{: codeblock}

Run the following command to export the NFS file set for the client CIDR:

```text
# mmnfs export add /gpfs/fs1/lsf --client "10.241.0.0/18(Access_Type=RW,SQUASH=NO_ROOT_SQUASH)" mmnfs: The NFS export was created successfully
```
{: codeblock}

Run the following command to update the routing on all Storage Scale CES cluster nodes:

```text
# ip route add 10.241.0.0/18 via 10.241.17.1 dev eth1
```
{: codeblock}

Run the following command to check whether the `NO_ROOT_SQUASH` is applied successfully:

```text
# mmnfs export list --nfsdefs /gpfs/fs1/lsf
Path Delegations Clients Access_Type Protocols Transport Squash ...
------------- ----------- ------------- ----------- --------- ---------
/gpfs/fs1/lsf NONE 10.241.0.0/20 RW 3,4 TCP NO_ROOT_SQUASH
```
{: codeblock}

When all the above steps are completed, you can use these endpoints as a common point for the LSF binaries to be shared with Scale.

```text
# default = [{ mount_path = "/mnt/lsf", nfs_share = "test-scale-poc-ces.cesscale.com:/gpfs/fs1/lsf" }, { mount_path = "/mnt/scale/tools", nfs_share = "test-scale-poc-ces.cesscale.com:/gpfs/fs1/tools" }, { mount_path = "/mnt/scale/data", nfs_share = "test-scale-poc-ces.cesscale.com:/gpfs/fs1/data" }]
```
{: codeblock}

For sharing the LSF binaries, you can still use the VPC file storage, but as the design the VPC file share supports a maximum of 250 nodes only. But with this solution it is suggested that when you use the Scale NFS, create a new file export for LSF as `/gpfs/fs1/lsf` and share the same mount point.
{: note}

* If it is necessary to use the default endpoints on the login node, then you need to update the NFS mount points with right CIDR ranges to be mounted on the login node. Since, the default NFS points are created with compute subnet range. All the subnets are part of the VPC. You might change the export list to point to /18, so that any nodes that are part of this VPC range can be able to mount even the default nodes.

```text
# mmnfs export change /gpfs/fs1/data --nfsadd "10.241.0.0/18(Access_Type=RW,SQUASH=no_root_squash)" 
# mmnfs: The NFS export was changed successfully.
```
{: codeblock}

```text
# mmnfs export change /gpfs/fs1/tools --nfsadd "10.241.0.0/18(Access_Type=RW,SQUASH=no_root_squash)" 
# mmnfs: The NFS export was changed successfully.
```
{: codeblock}

```pre
# mmnfs export list
Path                Delegations                 Clients
---------------------------------------------------------
/gpfs/fs1/data         NONE                10.241.0.0/20
/gpfs/fs1/data         NONE                10.241.0.0/18
/gpfs/fs1/tools        NONE                10.241.0.0/18
/gpfs/fs1/tools        NONE                10.241.0.0/20
/gpfs/fs1/lsf          NONE                10.241.0.0/18
```
{: codeblock}

This is expected result after running the command.

* Updating the squash permission property for the NFS export.

To enable {{site.data.keyword.scale_short}} integration with your {{site.data.keyword.spectrum_full}} cluster, change the squash permission property for the NFS export (for example, for the `/gpfs/fs1/lsf` export). Update the `SQUASH` property from **ROOT_SQUASH** to **NO_ROOT_SQUASH** so that the {{site.data.keyword.scale_short}} and {{site.data.keyword.spectrum_full}} clusters can share configurations.

For example, as the `root` user, connect to the {{site.data.keyword.scale_short}} CES cluster node and run the following `mmnfs export` commands to change the squash property for the **/gpfs/fs1/lsf** NFS export:

```text
# mmnfs export list --nfsdefs /gpfs/fs1/lsf
Path          Delegations Clients       Access_Type Protocols Transports Squash ...
------------- ----------- ------------- ----------- --------- ---------- ------------
/gpfs/fs1/lsf NONE        10.241.0.0/20 RW          3,4       TCP        ROOT_SQUASH

# mmnfs export change /gpfs/fs1/lsf --nfschange "10.241.0.0/20(Access_Type=RW,SQUASH=NO_ROOT_SQUASH)"
mmnfs: The NFS export was changed successfully.

# mmnfs export list --nfsdefs /gpfs/fs1/lsf
Path          Delegations Clients       Access_Type Protocols Transport Squash ...
------------- ----------- ------------- ----------- --------- --------- --------------
/gpfs/fs1/lsf NONE        10.241.0.0/20 RW          3,4       TCP       NO_ROOT_SQUASH
```
{: codeblock}

### Associating the {{site.data.keyword.scale_short}} security group ID with your {{site.data.keyword.spectrum_full_notm}} cluster
{: #integrate-scale-security-group-ID}

The {{site.data.keyword.cloud_notm}} cluster nodes need to access the NFS mount points that are created under the specific storage security group.

To determine and integrate your storage security group ID from {{site.data.keyword.scale_short}} cluster in {{site.data.keyword.cloud_notm}}:

1. Go to the resources list.
2. Search for the prefix that is used to create the {{site.data.keyword.scale_short}} cluster.
3. Copy the security group ID of the security group.
4. Update the ID under the {{site.data.keyword.spectrum_full}} clusters `storage_security_group_id` deployment input value.

When the {{site.data.keyword.spectrum_full}} cluster is active and the NFS shares are mounted (you can use the `df-h` command to verify), the {{site.data.keyword.spectrum_full}} nodes have access to the integrated {{site.data.keyword.scale_short}} cluster.
{:tip: .tip}
