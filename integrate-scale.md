---

copyright:
  years: 2025
lastupdated: "2025-10-08"

keywords: storage scale, spectrum lsf, integration
subcollection: hpc-ibm-spectrumlsf
deployment-url: https://cloud.ibm.com/catalog/content/ibm-spectrum-scale-d722b6b6-8bb5-4506-8f0f-03a5f05a3d6e-global
industry: Technology
version: v4.0.0

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
{: toc-industry="Technology"}
{: toc-version="v4.0.0"}

After deploying your Storage Scale cluster with CES, set up your {{site.data.keyword.spectrum_full_notm}} cluster to utilize the CES NFS mount points as a shared file storage solution.
{: shortdesc}

## Deploying your {{site.data.keyword.scale_short}} cluster
{: #scale-tile}

Deploy your {{site.data.keyword.scale_short}} cluster using the deployable architecture variant [Scale tile](https://cloud.ibm.com/catalog/content/ibm-spectrum-scale-d722b6b6-8bb5-4506-8f0f-03a5f05a3d6e-global).
Refer to the [{{site.data.keyword.scale_short}} documentation](/docs/storage-scale-da) for more information.

When you create this workspace during {{site.data.keyword.scale_short}} cluster deployment:
1. Select to use product version 4.0.0.
2. [Configure CES deployment values](/docs/storage-scale?topic=storage-scale-config-ces-integration-ldap-authentication#beforeyoubegin-config-ces) for your {{site.data.keyword.scale_short}} cluster by enabling the CES feature:
* Update the `total_protocol_cluster_instances` deployment value to be greater than or equal to **2** for high availability.
* Configure the necessary NFS mount points by updating the `filesets` value. This configuration creates independent file sets that act as NFS mount points for your {{site.data.keyword.spectrum_full}} cluster.
* Once the Scale cluster is successfully created, login to the CES node to run the following command.

By default, the automation creates the filesystem with **/gpfs/fs1** but if you have different filesystem name then provide your filesystem path.
{: tip}

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

* To create the LSF management node and compute worker nodes use the compute subnets (comp-pvt-1) from the Scale cluster. Provide the compute subnet ID under the `compute_subnet_id` parameter.

* To create the bastion/deployer and login nodes on the LSF, there is an existing **login subnet** created already as part of the Scale cluster. Provide that subnet ID under the `login_subnet_id` parameter. Using the **storage subnet** created from Scale is not advisable to use due to security issues, this approach ensures that the bastion/deployer and login node do not have a direct access to the Storage Scale nodes, which aligns with the planned architecture.

    You can use either login subnet or client subnet or protocol subnets which are created through the Scale cluster. You can pass either of them during deployment.
    The new subnet created should have the Public Gateway (PGW) attached, and this is required for the deployer node to clone the terraform code for the deployment process. For more information on how to attach the PGW, see [Working with subnets in VPC](https://cloud.ibm.com/docs/vpc?topic=vpc-subnets-configure&interface=ui).
    {: note}

* Provide the existing custom resolver ID under the `dns_custom_resolver_id` parameter. Since a custom resolver ID was already created under the Scale VPC, failing to provide this details cause the LSF deployment to fail.

* Provide the Storage Scale cluster storage security group ID under the `storage_security_group_id` parameter. This security group ID is required to establish the connection from the LSF cluster nodes to Storage Scale CES nodes from where the NFS mount points are exported.

* To use the Storage Scale CES NFS mount points on the LSF cluster nodes, ensure to pass the mount point details under the `custom_file_shares` parameter.

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

There are few configuration changes that need to be done to export a new NFS called `/gpfs/fs1/lsf`. The default mount points `/gpfs/fs1/data` and `/gpfs/fs1/tool` are created through the protocol subnet CIDR range. When the login subnet is created through a different CIDR then the default mount points cannot be accessed as the ranges are different and from the login CIDR range a new export should be created. Run the following commands:

```text
# mmcrfileset fs1 new_fileset --inode-space new
# mmlinkfileset fs1 new_fileset -J <provide the filesystem path>/new_fileset
```
{: codeblock}

Run the following command to export the NFS file set for the client CIDR:

```text
# mmnfs export add <provide the filesystem path>/lsf --client "10.241.0.0/18(Access_Type=RW,SQUASH=NO_ROOT_SQUASH)" mmnfs: The NFS export was created successfully
```
{: codeblock}

To update the routing configuration across all CES (Protocal) nodes, run the following command on each node:

```text
# ip route add 10.241.0.0/18 via <gateway_ip> dev eth1
```
{: codeblock}

    Example:

    ```text
    # ip route add 10.241.0.0/18 via 10.241.40.1 dev eth1
    ```
    {: codeblock}

    **Note:**
    The value "10.241.40.1" represents the gateway IP address for the eth1 interface.
    To identify the correct gateway IP on your system, log in to any of the protocol nodes and run the following command:

    ```text
    # ip route show dev eth1 | awk '/via/ {print $3}'
    ```
    {: codeblock}

    Use the IP address displayed in this command in place of <gateway_ip> in the route configuration command above.

Run the following command to check whether the `NO_ROOT_SQUASH` is applied successfully:

```text
# mmnfs export list --nfsdefs <provide the filesystem path>/lsf
Path Delegations Clients Access_Type Protocols Transport Squash ...
------------- ----------- ------------- ----------- --------- ---------
<provide the filesystem path>/lsf NONE 10.241.0.0/20 RW 3,4 TCP NO_ROOT_SQUASH
```
{: codeblock}

When all the above steps are completed, you can use these endpoints as a common point for the LSF binaries to be shared with Scale.

```text
# default = [{ mount_path = "/mnt/lsf", nfs_share = "test-scale-poc-ces.cesscale.com:<provide the filesystem path>/lsf" }, { mount_path = "/mnt/scale/tools", nfs_share = "test-scale-poc-ces.cesscale.com:<provide the filesystem path>/tools" }, { mount_path = "/mnt/scale/data", nfs_share = "test-scale-poc-ces.cesscale.com:<provide the filesystem path>/data" }]
```
{: codeblock}

For sharing the LSF binaries, you can still use the VPC file storage, but as the design the VPC file share supports a maximum of 250 nodes only. But with this solution it is suggested that when you use the Scale NFS, create a new file export for LSF as `/gpfs/fs1/lsf` and share the same mount point.
{: note}

* If it is necessary to use the default endpoints on the login node, then you need to update the NFS mount points with right CIDR ranges to be mounted on the login node. Since, the default NFS points are created with compute subnet range. All the subnets are part of the VPC. You might change the export list to point to /18, so that any nodes that are part of this VPC range can be able to mount even the default nodes.

```text
# mmnfs export change <provide the filesystem path>/data --nfsadd "10.241.0.0/18(Access_Type=RW,SQUASH=no_root_squash)" 
# mmnfs: The NFS export was changed successfully.
```
{: codeblock}

```text
# mmnfs export change <provide the filesystem path>/tools --nfsadd "10.241.0.0/18(Access_Type=RW,SQUASH=no_root_squash)" 
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
# mmnfs export list --nfsdefs <provide the filesystem path>/lsf
Path          Delegations Clients       Access_Type Protocols Transports Squash ...
------------- ----------- ------------- ----------- --------- ---------- ------------
<provide the filesystem path>/lsf NONE        10.241.0.0/20 RW          3,4       TCP        ROOT_SQUASH

# mmnfs export change <provide the filesystem path>/lsf --nfschange "10.241.0.0/20(Access_Type=RW,SQUASH=NO_ROOT_SQUASH)"
mmnfs: The NFS export was changed successfully.

# mmnfs export list --nfsdefs <provide the filesystem path>/lsf
Path          Delegations Clients       Access_Type Protocols Transport Squash ...
------------- ----------- ------------- ----------- --------- --------- --------------
<provide the filesystem path>/lsf NONE        10.241.0.0/20 RW          3,4       TCP       NO_ROOT_SQUASH
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
