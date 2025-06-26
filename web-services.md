---

copyright:
  years: 2025
lastupdated: "2025-06-26"

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

# IBM Spectrum LSF Web Services
{: #about-web-services}

IBM Spectrum LSF Web Services is introduced in Fix Pack 15, which provides a RESTful HTTP(s) interface for remotely interacting with LSF clusters. This helps users to programmatically submit, monitor, and manage jobs, enabling seamless integration of LSF workload management into custom applications, web portals, and automation workflows. This eliminates the need for direct command-line access and offers secure, scalable access through standard APIs.

## Configuring IBM Spectrum LSF Web Services
{: #config-web-services}

IBM Spectrum LSF Web Services is enabled by default with the LSF Suite installation. The service is installed and runs on the primary management node (usually Management Node 1).

By default, High Availability (HA) for Process Manager is not enabled. The service runs as a stand-alone instance on a single node. If the node becomes unavailable, then the Web Services will also be temporarily inaccessible.
{: note}

### Verifying Web Service installation:
{: #verify-web-service}

1. Check the status of the Web Service by running the command: `systemctl status lwsd`

2. Stop the Web Service by running the command: `systemctl stop lwsd`

3. Start the Web Service by running the command: `systemctl start lwsd`

## Configure LSF Web Service with the `lsf` client
{: #config-lsf-web-services}

1. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the following `ssh_to_management_node` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -J ubuntu@162.133.142.116 lsfadmin@10.241.16.6
    ```

2. Copy the `lsf` binary from “/opt/ibm/lsfsuite/lsf/10.1/linux3.10-glibc2.17-x86_64/bin/lsf” from the management node to the LSF client node at the path `/usr/local/bin/`.

3. Connect to your LSF client node and verify the LSF version by running:

    ```pre
    root@test-re1-bastion-ce46-001:~# lsf version
    version:  v0.0.1  
    commit:   0.0.1  
    root@test-re1-bastion-ce46-001:~#
    ```

4. On your client node, log in to the LSF Web Service using your existing OS username and password from the web service host:

    ```pre
    lsf cluster logon --username lsfadmin --password xxxxxxx --url http://test-re1-mgmt-1-9e6e-001:8088
    ```

    The password refers to your Application Center GUI password provided during cluster creation.
    {: note}

    `test-re1-mgmt-1-9e6e-001` is your Management Node 1, where the web service is running.
    {: note}

5. Once the configuration is complete, you will be able to run any lsf commands from the same client node.

    ```pre
    root@test-re1-bastion-ce46-001:~# lsf lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.

    My cluster name is test-re1
    My master name is test-re1-mgmt-1-9e6e-001.lsf.com
    root@test-re1-bastion-ce46-001:~#

    root@test-re1-bastion-ce46-001:~# lsf bhosts -w
    HOST_NAME          STATUS          JL/U    MAX  NJOBS    RUN  SSUSP  USUSP    RSV
    test-re1-comp-1-9e6e-001.lsf.com ok              -      8      0      0      0      0      0
    test-re1-mgmt-1-9e6e-001.lsf.com closed_Full     -      0      0      0      0      0      0
    test-re1-mgmt-1-9e6e-002.lsf.com closed_Full     -      0      0      0      0      0      0
    root@test-re1-bastion-ce46-001:~#
    root@test-re1-bastion-ce46-001:~# lsf bsub -n 5 sleep 10
    Job <228> is submitted to default queue <normal>.
    root@test-re1-bastion-ce46-001:~#
    root@test-re1-bastion-ce46-001:~# lsf bjobs -a -w
    JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
    227     lsfadmin DONE  normal     test-re1-mgmt-1-9e6e-001.lsf.com test-re1-comp-1-9e6e-001.lsf.com sleep 10   Jun 25 14:52
    228     lsfadmin DONE  normal     test-re1-mgmt-1-9e6e-001.lsf.com 5*test-re1-comp-1-9e6e-001.lsf.com sleep 10   Jun 25 15:04
    root@test-re1-bastion-ce46-001:~#
    root@test-re1-bastion-ce46-001:~#
    ```
