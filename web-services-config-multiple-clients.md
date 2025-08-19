---

copyright:
  years: 2025
lastupdated: "2025-08-19"

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

# Configuring LSF client for multiple IBM Spectrum LSF clusters
{: #config-multiple-lsf-clients}

LSF Web Services are configured to run over HTTPS and use port 8443 for secure communication. Since the management nodes are not directly accessible from external networks, SSH tunneling is set up to securely access the clusters from a local machine.

## Connecting to Cluster 1
{: #cluster1}

1. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `connect_to_web_services` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8448:localhost:8448 -J ubuntu@165.192.135.237 lsfadmin@10.241.0.4
    ```

2. Open another terminal and run the command to copy the `cacert.pem` file from the management node to your local system:

    ```pre
    scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@165.192.143.29 lsfadmin@10.241.0.4:/opt/ibm/lsfsuite/ext/ws/conf/https/cacert.pem /Users/test/Desktop/
    ```

3. Open another terminal and set up https certificate on Client or Localhost.

    ```pre
    test@MacBook-Pro WebService_Certs % lsf config set --cacert /Users/test/Desktop/cacert.pem
    OK
    test@MacBook-Pro WebService_Certs % 
    ```

4. Log in to the LSF cluster using HTTPS on port 8448.

    ```pre
    test@MacBook-Pro WebService_Certs % lsf cluster logon --username lsfadmin --
    password abc@123 --url https://localhost:8448
    OK
    test@MacBook-Pro WebService_Certs % 
    ```

    Here `lsfadmin` is the default LSF user and password is the “AppCenter” UI password.
    {: note}

## Connecting to Cluster 2
{: #cluster2}

1. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `connect_to_web_services` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8449:localhost:8448 -J ubuntu@165.192.135.237 lsfadmin@10.241.0.4
    ```

    Cluster-1 is already utilizing the 8448 locally. So, you can use any other port to access the WebService locally, like 8449 and so on.
    {: note}

2. Open another terminal and run the command to copy the `cacert.pem` file from the management node to your local system:

    ```pre
    scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@165.192.143.29 lsfadmin@10.241.0.4:/opt/ibm/lsfsuite/ext/ws/conf/https/cacert.pem /Users/test/Desktop/
    ```

3. Open another terminal and set up https certificate on Client or Localhost.

    ```pre
    test@MacBook-Pro WebService_Certs % lsf config set --cacert /Users/test/Desktop/cacert.pem
    OK
    test@MacBook-Pro WebService_Certs % 
    ```

4. Log in to the LSF cluster using HTTPS on port 8449.

    ```pre
    test@MacBook-Pro WebService_Certs % lsf cluster logon --username lsfadmin --password abc@123 --url https://localhost:8449
    OK
    test@MacBook-Pro WebService_Certs % 
    ```

    Here "lsfadmin" is the default LSF user and “AppCenter” is the UI password.
    {: note}

After successful configuration, validate and run the workloads on different clusters from Localhost.

1. List all the LSF clusters.

    ```pre
    test@MacBook-Pro ~ % lsf cluster list
    Default      Name                 Version                             URL
    *           abc-vnc      IBM Spectrum LSF 10.1.0.15, Apr 14 2025   https://localhost:8448
                abc-jul10    IBM Spectrum LSF 10.1.0.15, Apr 14 2025   https://localhost:8449
    test@MacBook-Pro ~ %
    ```

2. Verify the specific cluster details.

    ```pre
    test@MacBook-Pro ~ % lsf --cluster abc-jul10 lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
    My cluster name is abc-jul10
    My master name is abc-jul10-mgmt-1-2fe8-001.lsf.com
    test@MacBook-Pro ~ %
    test@MacBook-Pro ~ %
    test@MacBook-Pro ~ % lsf --cluster abc-vnc lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
    My cluster name is abc-vnc
    My master name is abc-vnc-mgmt-1-8ab5-001.lsf.com
    test@MacBook-Pro ~ %
    ```

3. Submit the workload to the specific cluster.

    ```pre
    test@MacBook-Pro ~ % lsf --cluster abc-vnc bsub -n 1 sleep 1
    Job <102> is submitted to default queue <normal>.
    test@MacBook-Pro ~ % lsf --cluster abc-vnc bjobs -a
    JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
    102     lsfadmi DONE  normal     abc-vnc-m abc-vnc-c sleep 1    Jul 16 07:10
    test@MacBook-Pro ~ %
    ```

4. List all the jobs from the specific cluster.

    ```pre
    test@MacBook-Pro ~ % lsf --cluster abc-jul10 bjobs -a
    No job found
    test@MacBook-Pro ~ %
    ```

5. Log out of the cluster connection.

    ```pre
    test@MacBook-Pro ~ % lsf cluster logout --url https://localhost:8448
    lsfadmin is logged out.
    test@MacBook-Pro ~ %
    ```
