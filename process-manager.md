---

copyright:
  years: 2025
lastupdated: "2025-06-25"

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

# About IBM Spectrum LSF Process Manager
{: #about-process-manager}

IBM Spectrum LSF Process Manager is a component of the IBM Spectrum LSF (Load Sharing Facility) suite that provides advanced workload scheduling and job management. It helps to automate, monitor, and control application workflows and dependencies across distributed computing environments. With process manager, users can define complex job flows using visual tools or scripting, enabling efficient resource usage and better throughput. For more information, see [IBM Spectrum LSF Process Manager documentation](https://www.ibm.com/docs/en/slpm/10.2.0?topic=administering-about-spectrum-lsf-process-manager).

## Configuring IBM Spectrum LSF Process Manager
{: #config-process-manager}

IBM Spectrum LSF Process Manager is enabled by default as part of the LSF suite deployment. The process manager is installed and runs on the primary management node (typically Management Node 1).

By default, High Availability (HA) for Process Manager is not enabled. The service runs as a stand-alone instance on a single node. If the node goes down, process manager and application center UI will become temporarily unavailable.
{: note}

## Checking IBM Spectrum LSF Process Manager
{: #check-process-manager}

The Process Notification Controller (PNC) is the core service of LSF Process Manager. This is responsible for managing and executing job flows.

Connect to the LSF management node through SSH. The details are available in the Schematics log output with the following `application_center_tunnel` instance.

`ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8443:10.241.0.7:8443 -L 6080:10.241.0.7:6080 -L 8444:10.241.0.7:8444 -J ubuntu@162.133.142.116 lsfadmin@10.241.16.6`

**Verifying the process manager installation:**

```pre
[lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ rpm -qa |grep "lsf-pm-server"
lsf-pm-server-10.2.0.15-25050119.x86_64
[lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$
```

**Checking the process manager runtime status:**

```pre
[root@test-re1-mgmt-1-9e6e-001 ~]# pmcadmin list | grep PNC
PNC            STARTED        1002459        8081           test-re1-mgmt-1-9e6e-001
[root@test-re1-mgmt-1-9e6e-001 ~]#
```

**Managing the process manager service:**

* Start Process Manager - `jadmin start`

* Stop Process Manager - `jadmin stop`

**Troubleshooting - PMC services not running:**

If the `pmcadmin list` shows services in a stop state, then it is possible that the ACD daemon (Application Control Daemon) is not running. To restart, run the command
`systemctl restart acd`

This daemon is responsible for managing and monitoring LSF Process Manager services such as PNC and WEBGUI.
