---

copyright:
  years: 2026
lastupdated: "2026-03-20"

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

Configuring the License Scheduler helps automate and optimize how licenses are allocated across your system.

The `enable_license_scheduler` variable determines whether the License Scheduler is enabled for the cluster. When set to true, the License Scheduler batch daemon (`bld`) runs on management node 2.
You can verify if the daemon is active by running the command `ps -ef | grep bld` on that node.
The `lsf.licensescheduler` file contains {{site.data.keyword.spectrum_full}} License Scheduler configuration information which is present in **/opt/ibm/lsf/confdirectory**.

## Pre-requisites
{: #pre-req}

* User must have their own license manager, such as **FlexNet** or **Reprise License Manager**.
* The node from which the job is submitted must have the `lmstat` or `rlmstat` commands.

## Procedure
{: #proc-lsf-sch}

### Parameters
{: #parameters}

1. List the License Scheduler hosts.

    By default, the daemon is running on **management node 2**. Users can add more nodes. The first listed node is the primary, and the remaining nodes act as secondary or backup hosts if the primary is unavailable.

2. Specify the file paths to the license‑manager command.

    a. If you are using **FlexNet**, specify the path to `lmutil` (or `lmstat`) command.

    For example, if lmstat is in `/etc/flexlm/bin`:

    ```text
    LMSTAT_PATH=/etc/flexlm/bin
    ```
    {: codeblock}

    b. If you are using **Reprise License Manager**, specify the path to `rlmutil` (or `rlmstat`) command.

    For example, if the commands are in `/etc/rlm/bin`:

    ```text
    RLMSTAT_PATH=/etc/rlm/bin
    ```
    {: codeblock}

3. Cluster mode can be set globally or for individual license features.

    a. If you are using cluster mode for all license features, define `CLUSTER_MODE=Y` in the **Parameters** section of `lsf. Licensescheduler`.

    b. If you are using cluster mode for some license features, define `CLUSTER_MODE=Y` for individual license features in the **Feature** section of `lsf.licensescheduler`.
    The Feature section setting of **CLUSTER_MODE** overrides the global Parameter section setting.

### Service domain
{: #service-domain}

In the **ServiceDomain** section of the `lsf.licensescheduler` file, configure the service domains by specifying the license server names and port numbers.

```text
Begin ServiceDomain
NAME=DesignCenterA
LIC_SERVERS=((1700@hostA))
End ServiceDomain
```
{: codeblock}

You must configure atleast one service domain for License Scheduler.
{: note}

## Configure license features
{: #config-license}

### Cluster mode
{: #cluster-mode}

1. Specify the feature name used by the license manager to identify the license type by setting the **NAME** parameter.

2. Optionally, define an alias by setting:

    a. `LM_LICENSE_NAME` to the license‑manager feature name and
    b. `NAME` to the LSF License Scheduler feature name

3. `LM_LICENSE_NAME` is mandatory if the token name differs from the license‑manager feature name, or if the feature name starts with a number or contains a hyphen (‑), which are not supported in LSF.

    **For example:**

    The license manager feature name **201-AppZ** is not supported in LSF because the feature name starts with a number and contains a hyphen. Therefore, define **AppZ201** as an alias of the 201-AppZ license manager feature name as follows:
    ```text
    NAME=AppZ201
    LM_LICENSE_NAME=201-AppZ
    ```
    {: codeblock}

4. Set the service domains in the feature section using the command:

```text
CLUSTER_DISTRIBUTION=service_domain(cluster_name share)
```
{: codeblock}

### Project mode
{: #project-mode}

1. If the user wants to use the project mode, then define:

    ```pre
    Begin Projects
    PROJECTS
    myProject1
    myProject2
    myProject3
    End Projects
    ```

2. Specify the feature name used by the license manager to identify the license type by setting the **NAME** parameter.

3. Optionally, define an alias by setting:

    a. `LM_LICENSE_NAME` to the license‑manager feature name and
    b. `NAME` to the LSF License Scheduler feature name

4. `LM_LICENSE_NAME` is mandatory if the token name differs from the license‑manager feature name, or if the feature name starts with a number or contains a hyphen (‑), which are not supported in LSF.

    **For example:**

    The license manager feature name **201-AppZ** is not supported in LSF because the feature name starts with a number and contains a hyphen. Therefore, define **AppZ201** as an alias of the 201-AppZ license manager feature name as follows:

    ```text
    NAME=AppZ201
    LM_LICENSE_NAME=201-AppZ
    ```
    {: codeblock}

5. A distribution policy defines the license fair share policy in the format:

```text
DISTRIBUTION = ServiceDomain (project1 share_ratio project2 share_ratio ...)
```
{: codeblock}

### Steps - After configuration
{: #after-config}

Once you make the configuration changes, you must reconfigure License Scheduler to apply the changes using the following commands:

1. Run `bld -C` - to test for configuration errors.
2. Run `bladmin reconfig` - reconfigures LSF License Scheduler.
3. Run `badmin mbdrestart` - restarts the mbatchd daemon.
4. Run `lsadmin reconfig` - reconfigure the LIM.

## Submitting License Scheduler jobs
{: #submit-jobs}

When you submit an LSF License Scheduler, you must reserve the license with resource usage (`rusage`) by running the command `bsub -R "rusage"`.

1. The following command submits a job named **myjob** to license project Lp1 and requests one AppB license:

    ```text
    bsub -R "rusage[AppB=1]" -Lp Lp1 myjob
    ```
    {: codeblock}

2. The following command submits a job named **myjob** and requests one AppC license:

    ```text
    bsub -R "rusage[AppC=1]" myjob
    ```
    {: codeblock}

    where:
    * rusage[NAME=required number of license_token].
    * this NAME is defined in feature section.

### Commands
{: #commands}

* `bladmin reconfig`: Reconfigures LSF License Scheduler.
* `blhosts`: Prints the names of all the hosts that are running the LSF License Scheduler bld daemon.
* `blinfo`: displays information about the distribution of licenses that are managed by LSF License Scheduler.
* `blstat`: Displays license usage statistics for LSF License Scheduler.
* `blstat -t token_name`: Shows only information about specified license tokens.

### Outputs
{: #output}

1. When the job has started and the token is reserved for job:

    ```text
    [lsfadmin@management_node2 conf]$ blstat -t <token_name>
    FEATURE: <token_name>@<cluster_name>
    SERVICE_DOMAIN: <service_domain_name>
    TOTAL_TOKENS: 10  TOTAL_ALLOC: 10  TOTAL_USE: 1  OTHERS: 0
    CLUSTER     SHARE   ALLOC   TARGET   INUSE    RESERVE   OVER   PEAK   BUFFER   FREE   DEMAND
    <cluster_name> 100.0%  10       10       0        1         0      1      -        9       0
    ```
    {: codeblock}

2. When the job is using the license token:

    ```text
    [lsfadmin@management_node2 conf]$ blstat -t <token_name>
    FEATURE: <token_name>@<cluster_name>
    SERVICE_DOMAIN: <service_domain_name>
    TOTAL_TOKENS: 10  TOTAL_ALLOC: 10  TOTAL_USE: 1  OTHERS: 0
    CLUSTER      SHARE   ALLOC   TARGET   INUSE   RESERVE   OVER   PEAK   BUFFER   FREE   DEMAND
    <cluster_name> 100.0%   10       10       1        0        0      1       -       9       0
    ```
    {: codeblock}

3. If a job is requesting for a token but all tokens are reserved:

    ```text
    [lsfadmin@management_node2 conf]$ blstat -t <token_name>
    FEATURE: <token_name>@<cluster_name>
    SERVICE_DOMAIN: <service_domain_name>
    TOTAL_TOKENS: 10  TOTAL_ALLOC: 10  TOTAL_USE: 10  OTHERS: 0
    CLUSTER      SHARE   ALLOC   TARGET   INUSE   RESERVE   OVER   PEAK   BUFFER   FREE   DEMAND
    <cluster_name> 100.0%   10       10       0       10        0     10       -       0       1
    ```
    {: codeblock}

If an ldap user is present, then user can run jobs as ldap user also.
{: note}
