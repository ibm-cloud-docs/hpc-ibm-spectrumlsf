---

copyright:
  years: 2025
lastupdated: "2025-07-10"

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

# Accessing LSF Application Center
{: #accessing-lsf-gui}

You can use multiple methods of accessing LSF Application Center from an [LSF Application Center supported browser](https://www.ibm.com/docs/en/slac/10.2.0?topic=requirements-system-102-fix-pack-14#pac_sysreqs_10.2.0.14__title__6){: external}.
{: shortdesc}

## Gathering IP addresses
{: #gathering-ip-addresses}

Regardless of which method you use to access LSF Application Center, first collect the IP addresses of your compute nodes and the floating IP address that is attached to your bastion node:

1. In the {{site.data.keyword.cloud_notm}} console, select Projects > _project_name_ > Configurations > _project_configuration_name_ > Resources tab, and click the link for the workspace.
2. On the workspace page, click the _Resources_ tab. In the list of Terraform resources, click the `management_host` resource link, which opens a new window with a list of the VSIs that were provisioned.
3. On the Virtual server instances for VPC page, locate the IP address for `<cluster_prefix>-mgmt-1`, which is the management IP address that you need.
4. Locate the floating IP address that is attached to the bastion node.

## Logging in to LSF Application Center
{: #accessing-lsf-application-center}

You can access the LSF Application Center directly:

1. Open a new command line console and run the following command:

    ```console
    application_center_tunnel = "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8443:pac.hpcaas.com:8443 -L 6080:pac.hpcaas.com:6080 -L 8444:pac.hpcaas.com:8444 -J ubuntu@<floating_IP_address> lsfadmin@<login_VSI_IP_address>" ... ..."
    ```
    {: codeblock}

    Where `<floating_IP_address>` is the floating IP address for the bastion node and `<management_node_IP_address>` is the IP address for the management node.

    You can copy the command to access LSF Application Center from the output section of your workspace; look for `application_center`.
    {: tip}

2. Open a browser:
    * If you are on your local host, access https://localhost:8443.
    * If you enabled a [VPN connection](/docs/iaas-vpn?topic=iaas-vpn-about-iaas-vpn) or a direct link among your on-premises facilities and the VPC, access http://pac.<domain_name>, where `<domain_name>` is your {{site.data.keyword.cloud}} HPC cluster domain name.

3. Log in to LSF Application Center, by using the default user lsfadmin and the password that you configured when you deployed your {{site.data.keyword.cloud_notm}} cluster (for example, Admin@123).

    If LDAP is enabled, you can access the LSF Application Center:
    * by using the LDAP username and password that you configured during {{site.data.keyword.cloud}} HPC cluster deployment.
    * by using an existing LDAP username and password.

Clear the browsers cache if you encounter slowness in loading or accessing LSF Application Center.
{: tip}

## Accessing LSF Application Center through VNC (remote) consoles
{: #accessing-lsf-application-center-vnc}

You can also access LSF Application Center by using a remote console, which opens a remote desktop on the LSF Application Center server through Virtual Network Computing (VNC):

1. In LSF Application Center, select the Workload tab.
2. Perform one of the following actions:

    * To submit a custom application job from LSF Application Center, click GEDIT. Wait for the remote console to open, choose the appropriate language and security requirement option, and click Next to access the remote console.

    * To open a new remote console, select VNC Consoles > Open my console to access the remote console.

When you deploy the {{site.data.keyword.cloud_notm}} HPC cluster with LSF Application Center and enable high availability by using a self-signed certificate, you need to accept the certificates for both the LSF Application Center port (8443) for https://pac.<domain_name>:8443, and the VNC port (6080) for https://pac.<domain_name>:6080.
{:note: .note}

## Accessing LSF Application Center by using SSH tunneling from a bastion host
{: #accessing-lsf-application-center-ssh-bastion}

When you deploy the {{site.data.keyword.cloud}} HPC cluster with LSF Application Center and high availability that is enabled, the output section provides guidance about accessing LSF Application Center with an SSH tunnel. Note the `application_center_tunnel` and `application_center_url_NOTE` lines in the following example output:

```console
application_center_tunnel = "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8443:pac.hpcaas.com:8443 -L 6080:pac.hpcaas.com:6080 -J ubuntu@<floating_IP_address> lsfadmin@<login_VSI_IP_address>"
application_center_url = "https://pac.<dommain_name>:8443"
application_center_url_NOTE = "you may need '127.0.0.1 pac pac.<domain-name>' in your /etc/hosts, to let your browser use the ssh tunnel"
```
{: codeblock}

Where `<floating_IP_address>` is the floating IP address for the bastion node, `<login_VSI_IP_address>` is the IP address for the login node, and `<domain_name>` is your {{site.data.keyword.cloud}} HPC cluster domain name.

You need '127.0.0.1 pac.<domain-name>' on /etc/hosts of your local system where the connection is established so that your browser uses the ssh tunnel.
{: note}

To access LSF Application Center by creating a port-forwarding SSH tunnel from your bastion host:

1. Edit the `./etc/hosts` file on your host to add this line to the file:
    ```console
    127.0.0.1 pac pac.<domain-name>
    ```
    {: codeblock}

    For example:
    ```console
    127.0.0.1 pac pac.mydomain.com
    ```
    {: codeblock}

2. Customize your `./ssh/config` file for SSH tunnelling and port forwarding for the login VSI. This example shows settings for a bastion and a login VSI:
    ```console
    Host clusterX-vsi-for-bastion
        Hostname <bastion_floating_IP_address>
        ForwardAgent yes
        IdentityFile ~/.ssh/my-ssh-key
        StrictHostKeyChecking no
        User ubuntu
    Host clusterX-vsi-for-login
        Hostname clusterX-vsi-for-login-001.hpcaas.com
        ForwardAgent yes
        LocalForward 8443 pac.<domain_name>:8443
        LocalForward 8444 pac.hpcaas.com:8444
        LocalForward 6080 pac.<domain_name>:6080
        IdentityFile ~/.ssh/my-ssh-key
        StrictHostKeyChecking no
        User lsfadmin
        ProxyJump ubuntu@<bastion_floating_public_IP_address>
    ```
    {: codeblock}

3. Establish an SSH connection to the login VSI. For example, to connect to the clusterX-vsi-for-login VSI, run:
    ```console
    ssh clusterX-vsi-for-login
    ```
    {: codeblock}

4. Open a browser and access https://pac.<domain_name>:8443. For example, https://pac.mydomain:8443.
