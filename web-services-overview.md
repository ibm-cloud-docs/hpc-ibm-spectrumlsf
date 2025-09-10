---

copyright:
  years: 2025
lastupdated: "2025-09-10"

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

# About IBM Spectrum LSF Web Services
{: #about-web-services}

IBM Spectrum LSF Web Services provides a RESTful HTTP(s) interface for remotely interacting with LSF clusters. This helps users to programmatically submit, monitor, and manage jobs, enabling seamless integration of LSF workload management into custom applications, web portals, and automation workflows. Spectrum LSF Web Services is introduced in Fix Pack 15. This eliminates the need for direct command-line access and offers secure, scalable access through standard APIs.

## Configuring IBM Spectrum LSF Web Services
{: #config-web-services}

IBM Spectrum LSF Web Services is enabled by default with the LSF Suite installation. The service is installed and runs on the secondary management node (typically Management Node 2). If the cluster is created with only one management node, Web Services are configured on that same node.

By default, High Availability (HA) for Web Services is not enabled. The service runs as a stand-alone instance on a single node. If the node becomes unavailable, then the Web Services will also be temporarily inaccessible.
{: note}

## Checking the IBM Spectrum LSF Web Services
{: #checking-lsf-web-services}

1. Connect to the LSF management node using SSH. The connection details are provided in the Schematics log output under the `ssh_to_management_node` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<Bastion_Node_IP> lsfadmin@<Management_Node_IP>
    ```

    Note:
    * For clusters with a single management node, all the configurations run on that same node. You can use the `ssh_to_management_node` tunnel for validation.

    * For clusters with multiple management nodes, Web Services are installed and configured on the second management node. In this case, replace <Management_Node_IP> with the IP address of management node 2.

2. Following are the commands to manage the Web Services:

* To check the status of the Web Service - `systemctl status lwsd`

* To start the Web Service - `systemctl start lwsd`

* To stop the Web Service - `systemctl stop lwsd`
