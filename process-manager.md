---

copyright:
  years: 2025
lastupdated: "2025-09-24"

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

# About Process Manager
{: #about-process-manager}

IBM Spectrum LSF Process Manager is a component of the IBM Spectrum LSF (Load Sharing Facility) suite that provides advanced workload scheduling and job management. This helps users to automate, monitor, and control application workflows and dependencies across distributed computing environments. With process manager, users can define complex job flows using visual tools or scripting, enabling efficient resource usage and better throughput. For more information, see [IBM Spectrum LSF Process Manager documentation](https://www.ibm.com/docs/en/slpm/10.2.0?topic=administering-about-spectrum-lsf-process-manager).

## Configuring IBM Spectrum LSF Process Manager
{: #config-process-manager}

IBM Spectrum LSF Process Manager is enabled by default with the LSF Suite installation. The service is installed and runs on the secondary management node (typically Management Node 2).

If the cluster is created with only one management node, Process Manager are configured on that same node.

By default, High Availability (HA) for Process Manager is not enabled. The service runs as a stand-alone instance on a single node. If the node becomes unavailable, then the Process Manager will also be temporarily inaccessible.
{: note}

## Checking IBM Spectrum LSF Process Manager
{: #check-process-manager}

The Process Notification Controller (PNC) is the core service of LSF Process Manager. This is responsible for managing and executing job flows.

1. Connect to the LSF management node through SSH. The details are available in the Schematics log output with `ssh_to_management_node` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -J ubuntu@<Bastion_Node_IP> lsfadmin@<Management_Node_IP>
    ```

2. To verify the process manager installation, run the below commands:

    ```pre
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ rpm -qa |grep "lsf-pm-server"
    lsf-pm-server-10.2.0.15-25050119.x86_64
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$
    ```

3. To check the process manager runtime status, run the below commands:

    ```pre
    [root@test-re1-mgmt-1-9e6e-001 ~]# pmcadmin list | grep PNC
    PNC            STARTED        1002459        8081           test-re1-mgmt-1-9e6e-001
    [root@test-re1-mgmt-1-9e6e-001 ~]#
    ```

4. Manage the the process manager service by:

    * Start Process Manager - `jadmin start`

    * Stop Process Manager - `jadmin stop`

5. Troubleshoot - If the `pmcadmin list` shows services in a stop state, then it is possible that the Application Control Daemon (ACD) is not running. To restart, run the command: `systemctl restart acd`

This daemon is responsible for managing and monitoring LSF Process Manager services such as PNC and WebGUI.
