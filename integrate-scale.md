---

copyright:
  years: 2025
lastupdated: "2025-02-13"

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

After you deploy your {{site.data.keyword.scale_short}} cluster with CES, deploy your {{site.data.keyword.spectrum_full}} cluster and integrate the {{site.data.keyword.scale_short}} values so that {{site.data.keyword.spectrum_full}} uses {{site.data.keyword.scale_short}} as the shared file storage solution.
{: shortdesc}

## Deploying your {{site.data.keyword.scale_short}} cluster
{: #scale-tile}

The solution provides flexibility by supporting both Storage scale NFS mount points and VPC file storage for LSF clusters. Below are the key considerations and guidelines:

1. VPC File Storage Limitations:
* The VPC file storage supports up to 250 dynamic nodes.
* This limitation is by design and cannot be exceeded.
2. Default storage scale NFS Mount Point
* By default, a custom NFS mount point is provided with the path "/mnt/scale/tools".
* Users must pass additional NFS mount points using the nfs_share parameter.
3. Sample Mount Configuration:
```text
[ { mount_path = "/mnt/vpcstorage/tools", size = 100, iops = 2000 }, { mount_path = "/mnt/vpcstorage/data", size = 100, iops = 6000 }, { mount_path = "/mnt/scale/tools", nfs_share = "" ]
```
{: codeblock}

4. Scaling Beyond 250 Nodes:
* For environments requiring more than 250 dynamic nodes, it is recommended to create a new NFS share from the storage cluster.
* Provide the path as input in the following format:hcl
`{ mount_path = "/mnt/scale/lsf", nfs_share = "" }`

5. Configure the `vpc_compute_subnet` and `vpc_compute_cluster_private_subnets_cidr_block` values. These subnet-related values are used to export the NFS mount that is needed for the {{site.data.keyword.spectrum_full}} cluster. The NFS mount can be an {{site.data.keyword.spectrum_full}} cluster subnet that you use for management and dynamic compute nodes. Note the values of `vpc_compute_subnet` and `vpc_compute_cluster_private_subnets_cidr_block`, as you use them when you deploy your {{site.data.keyword.spectrum_full}} cluster.

### Verifying the {{site.data.keyword.scale_short}} cluster
{: #scale-verify}

After your {{site.data.keyword.scale_short}} cluster is up and running, as the `root` user on one of the {{site.data.keyword.scale_short}} nodes, you can verify the health of the cluster by running the [mmhealth cluster show](https://www.ibm.com/docs/en/storage-scale/5.2.2?topic=reference-mmhealth-command) command. For example:

```text
# mmhealth cluster show
Component           Total         Failed       Degraded        Healthy          Other
-------------------------------------------------------------------------------------------
NODE                   12              0              0             11              1
GPFS                   12              0              0             11              1
NETWORK                12              0              0             12              0
FILESYSTEM              1              0              0              1              0
DISK                   41              0              0             41              0
FILESYSMGR              1              0              0              1              0
CES                    12              0              0             12              0
CESCLUSTER              1              0              0              1              0
GUI                     1              0              0              1              0
PERFMON                12              0              0             12              0
THRESHOLD              12              0              0             12              0
```
{: codeblock}

To verify the {{site.data.keyword.scale_short}} NFS exports, as the `root` user, connect to the {{site.data.keyword.scale_short}} CES cluster node and run the `mmnfs export list` command to list all current NFS exports. For example:

```text
# mmnfs export list

Path               Delegations       Clients
-------------------------------------------------
/gpfs/fs1/lsf      NONE              10.241.0.0/20
/gpfs/fs1/tools    NONE              10.241.0.0/20
/gpfs/fs1/data     NONE              10.241.0.0/20
```
{: codeblock}

When the {{site.data.keyword.spectrum_full}} cluster is active and the NFS shares are mounted (you can use the `df-h` command to verify), the {{site.data.keyword.spectrum_full}} nodes have access to the integrated {{site.data.keyword.scale_short}} cluster.
{:tip: .tip}

### Updating the squash permission property for the NFS export
{: #scale-update-squash}

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

## Integrating {{site.data.keyword.scale_short}} values for your {{site.data.keyword.spectrum_full_notm}} cluster deployment
{: #integrate-scale-and-hpc}

After you deploy and verify your Storage Scale cluster, you deploy your [IBM Spectrum LSF cluster](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-deploy-architecture&interface=ui).

### LSF Cluster Subnet Requirements
{: #integrate-lsf-cluster}

The LSF cluster setup requires two distinct subnets:

1. Bastion/Login Subnet:
    * This subnet is used for deploying the login and bastion nodes.
2. Cluster Management/Compute Subnet:
    * This subnet is for deploying the cluster management and compute nodes.

### Guidelines for Subnet Configuration
{: #integrate-guidelines}

1. Creating the Management/Compute Nodes:
    * Use the `cluster_subnet_ids` parameter to specify the compute subnet.
    * This subnet should already exist in the storage scale cluster.
    * The subnet name typically follows the pattern `<cluster_prefix>-comp-pvt-1`.

2. Creating the Login Node:
    * Manually create a new subnet in the storage cluster and provide its ID under `login_subnet_id`.
    * This subnet is used to deploy both the login and bastion nodes.
    * The solution also supports reusing an existing bastion node, if available.

### Important Considerations
{: #integrate-considerations}

1. When a storage scale cluster is created, two additional subnets are automatically generated:
    * proto-pvt-1
    * stg-pvt-1
2. Security note:
    * Avoid using these subnets for the login and bastion nodes, doing so grants direct access to storage, which conflicts with the secure access design principles.

### NFS Mount Points for Login Node
{: #nfs-login-node}

If a new subnet is created for the login node, NFS mount points can be added to share scale storage as required by the login node.
This setup ensures a secure, scalable, and properly configured environment for the LSF cluster.

To make sure that the {{site.data.keyword.spectrum_full}} cluster uses {{site.data.keyword.scale_short}} (instead of {{site.data.keyword.filestorage_vpc_full}}) as your shared file storage system, update a list of values for the {{site.data.keyword.spectrum_full}} cluster deployment so that the {{site.data.keyword.scale_short}} and {{site.data.keyword.spectrum_full}} deployments are integrated:

1. Set the `cluster_subnet_id` and `vpc_cluster_private_cidr_blocks` deployment value as the same values as the `vpc_compute_subnet` and `vpc_compute_cluster_private-subnets_cidr_block` values for your {{site.data.keyword.scale_short}} deployment.

2. Derive the `custom_file_shares` value for your {{site.data.keyword.spectrum_full}} deployment from the `filesets` value of your {{site.data.keyword.scale_short}} deployment. The attributes of the `custom_file_shares` value are as follows:

    * `mount_path` is the path where you expect the NFS exports to be mounted on the {{site.data.keyword.spectrum_full}} cluster (the default is **/mnt/scale/tools**). The `custom_file_shares` value contains a list of `mount_path` values, and one of these `mount_path` values must be the **/mnt/lsf** mount to indicate to use {{site.data.keyword.scale_short}} as the shared file storage for the {{site.data.keyword.spectrum_full}} cluster.

    * `nfs_share` is the NFS file share between {{site.data.keyword.scale_short}} and {{site.data.keyword.spectrum_full}}. It is derived by concatenating the following criteria:
        1. The `resource_prefix` value for {{site.data.keyword.scale_short}}, such as **LSF**.
        2. A hyphen (-).
        3. The text **ces**.
        4. A dot (.).
        5. The `vpc_protocol_cluster_dns_domain` values, such as **cesscale.com:/gpfs/fs1/lsf**.

        Example `nfs_share` value:

        ```text
        nfs_share = "hpc-ces.cesscale.com:/gpfs/fs1/lsf"
        ```
        {: codeblock}

        When you set the `nfs_share` as described, the `size` and `iops` values are not necessary and are ignored.

    Example of the entire `custom_file_shares` value, with `mount_path` and `nfs_share`:

    ```text
    custom_file_shares = [
    { mount_path = "/mnt/lsf", nfs_share = "hpc-ces.cesscale.com:/gpfs/fs1/lsf" },
    { mount_path = "/mnt/scale/tools", nfs_share = "hpc-ces.cesscale.com:/gpfs/fs1/tools" },
    { mount_path = "/mnt/scale/data", nfs_share = "hpc-ces.cesscale.com:/gpfs/fs1/data" }
    ]
    ```
    {: codeblock}

3. Provide the DNS custom resolver ID from the IBM Spectrum Storage Scale deployment. As you are using the existing VPC for the LSF deployment, it is required to provide the DNS resolver ID. If the resolver ID is not provided, then the LSF deployment tries to create a new resolver ID and attaches to the existing VPC, while the VPC from the storage deployment already has the resolver ID attached.

### Extending {{site.data.keyword.scale_short}} to a login node and subnet
{: #integrate-scale-adavanced}

You can optionally extend your {{site.data.keyword.scale_short}} shared file storage to your login node and subnet as follows:

1. Connect to one of the {{site.data.keyword.scale_short}} CES cluster nodes and switch to the `root` user.

2. (Optional) If you want to create an NFS share with values other than the values specified int eh {{site.data.keyword.scale_short}} `root` value, create and link an independent file set for the give file system:

    ```text
    # mmcrfileset fs1 new_fileset --inode-space new
    # mmlinkfileset fs1 new_fileset -J /gpfs/fs1/new_fileset
    ```
    {: codeblock}

3. Export the file set as the NFS export for the client CIDR:

    ```text
    # mmnfs export add /gpfs/fs1/lsf --client "10.241.0.0/18(Access_Type=RW,SQUASH=NO_ROOT_SQUASH)"
    mmnfs: The NFS export was created successfully
    ```
    {: codeblock}

4. Update routing by running the following command on all {{site.data.keyword.scale_short}} CES cluster nodes:

    ```text
    # ip route add 10.241.0.0/18 via 10.241.17.1 dev eth1
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
