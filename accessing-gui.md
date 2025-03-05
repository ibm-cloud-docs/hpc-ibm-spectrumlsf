---

copyright:
  years: 2025
lastupdated: "2025-03-05"

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

# Accessing the GUI
{: #accessing-gui}

After the cluster setup is complete, you can monitor the resources and status of the service directly from the {{site.data.keyword.spectrum_full_notm}} Application Center GUI. For more information about the GUI, see the [LSF Application Center Web Services product documentation](https://www.ibm.com/docs/en/slac/10.2.0?topic=lsf-application-center-web-services){: external}.
{: shortdesc}

## Before you begin
{: #before-you-begin}

Before you access the LSF Application Center GUI, review the following considerations and requirements:

* Initial setup must be done from your local system.
* Access the GUI with the same SSH key that was provided for cluster creation.
* It is recommended to use the Safari browser to access the GUI.
* If you encounter a delay in loading or accessing the GUI, clear the browsers cache.
* You can open Application Center GUIs on the 8443 port.

## Gathering IP addresses
{: #gathering-ip-address}

To access the GUI, you must collect the IP addresses of the compute nodes and the floating IP address that is attached to the login node.

1. In the {{site.data.keyword.cloud_notm}} console, go to Schematics > Workspaces, and choose your workspace.
2. On the workspace page, click the _Resources_ tab. In the list of Terraform resources, click the `management_host` resource link, which opens a new window with a list of the virtual server instances that were provisioned.
3. On the Virtual server instances for VPC page, locate the IP address for `<cluster_prefix>-login-host-0`, which is the management IP address that you need.
4. In addition to that IP address, pick up the floating IP address that is attached to the login node.

## Accessing the LSF Application Center GUI
{: #accessing-lsf-application-center-gui}

Complete the following steps to access the LSF Application Center:

1. Open a new command line terminal.

2. Run the following command to access the LSF Application Center GUI:

    ```
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8443:10.241.0.10:8443 -L 6080:10.241.0.10:6080 -L 8444:10.241.0.10:8444 -J ubuntu@{bastion_node_ip} lsfadmin@{login_host_ip}
    ```
    {: pre}

    Where `login_host_ip` needs to be replaced with the login node IP address that is associated with `<cluster_prefix>-login-host-0` and `FLOATING_IP_ADDRESS` needs to be replaced with the bastion node floating IP address. To find the management and login node IPs, see the instructions for [Gathering IP addresses](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-accessing-gui&interface=ui#gathering-ip-address).

    Example:
    ```
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8443:10.241.0.10:8443 -L 6080:10.241.0.10:6080 -L 8444:10.241.0.10:8444 -J ubuntu@52.116.124.34 lsfadmin@10.241.16.5
    ```
    {: pre}

3. Open a browser on your local system and run https://localhost:8443.
4. To access the LSF Application Center GUI, enter the default user as `lsfadmin` and enter the password that you configured when you created your workspace.
