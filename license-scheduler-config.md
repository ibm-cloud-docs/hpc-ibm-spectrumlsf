---

copyright:
  years: 2026
lastupdated: "2026-03-16"

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

# Configuring License Scheduler
{: #config-cluster-mode}

In cluster mode, licenses are distributed across LSF clusters, allowing each cluster to schedule jobs, allocating licenses to projects within the cluster, and preempt jobs.

## Pre-requisites
{: #pre-req}

* User must have their own license manager, such as **FlexNet** or **Reprise License Manager**.

* License Scheduler decides whether a job can be started based on the license availability.

* The main file to work is `lsf.licensescheduler` file present in `/opt/ibm/lsf/conf` directory.

## Procedure
{: #proc-lsf-sch}

1. Cluster mode can be set globally or for individual license features.

    a. If you are using cluster mode for all license features, define `CLUSTER_MODE=Y` in the **Parameters** section of `lsf. Licensescheduler`.

    b. If you are using cluster mode for some license features, define `CLUSTER_MODE=Y` for individual license features in the **Feature** section of `lsf.licensescheduler`.
    The Feature section setting of **CLUSTER_MODE** overrides the global Parameter section setting.

2. List the License Scheduler hosts.

    By default, the daemon is running on management node 2. Users can add more nodes. The first listed node is the primary, and the remaining nodes act as secondary or backup hosts if the primary is unavailable.

3. Specify the file paths to the license‑manager command being used.

    a. If you are using FlexNet, specify the path to the `lmutil` (or `lmstat`) command.

    For example, if lmstat is in `/etc/flexlm/bin`:

    ```text
    LMSTAT_PATH=/etc/flexlm/bin
    ```
    {: codeblock}

    b. If you are using Reprise License Manager, specify the path to the `rlmutil` (or `rlmstat`) command.

    For example, if the commands are in `/etc/rlm/bin`:

    ```text
    RLMSTAT_PATH=/etc/rlm/bin
    ```
    {: codeblock}

4. In the ServiceDomain section of the `lsf.licensescheduler` file, configure the service domains by specifying the license server names and port numbers.

    A service domain is a group of one or more license servers. You must configure atleast one service domain for License Scheduler.
    {: note}

5. Specify the license server hosts for that domain, including the host name and license manager port number.

## Configure license features
{: #config-license}

1. Specify the feature name used by the license manager to identify the license type by setting the **NAME** parameter.

2. Optionally, define an alias by setting `LM_LICENSE_NAME` to the license‑manager feature name and `NAME` to the LSF License Scheduler feature name.

3. Define `LM_LICENSE_NAME` only if the token name differs from the license‑manager feature name, or if the feature name starts with a number or contains a hyphen (‑), which are not supported in LSF.

4. Restart to implement configuration changes:

    1. Run `bladmin reconfig` to restart the bld.
    2. Run `badmin mbdrestart` to restart each LSF cluster.
