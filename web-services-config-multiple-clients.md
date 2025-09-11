---

copyright:
  years: 2025
lastupdated: "2025-09-11"

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

LSF Web Services are configured to run over HTTPS and use port 8448 for secure communication. Since the management nodes are not directly accessible from external networks, SSH tunneling is set up to securely access the clusters from a local machine.

## Connecting to Cluster 1
{: #cluster1}

1. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `web_service_tunnel` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8448:10.241.0.5:8448 -J ubuntu@162.133.143.84 lsfadmin@10.241.16.6
    ```

2. Open another terminal and run the command to copy the **cacert.pem** file from the management node to your local system:

    ```pre
    scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<Bastion_Node_IP> lsfadmin@<Management_Node_IP>:/opt/ibm/lsfsuite/ext/ws/conf/https/cacert pem /Users/test/Desktop/
    ```

    Note:

    * For clusters with a single management node, all configurations run on that same node. You can use the "ssh_to_management_node" tunnel for validation.

    * For clusters with multiple management nodes, Web Services are installed and configured on the second management node. In this case, replace <Management_Node_IP> with the IP address of management node 2.

3. Open another terminal and set up https certificate on Client or Localhost.

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf config set --cacert /Users/test/Desktop/cacert.pem
    OK
    test@abc-MacBook-Pro WebService_Certs % 
    ```

4. Log in to the LSF cluster using HTTPS on port 8448.

    ```pre
    test@abc-MacBook-Pro bin % ibmcloud lsf cluster logon --username lsfadmin --url https://localhost:8448
    Password>
    OK
    test@abc-MacBook-Pro bin %  
    ```

    The default LSF user is lsfadmin. When prompted for a password, enter the Application Center password that was provided during the cluster creation.
    {: note}

## Connecting to Cluster 2
{: #cluster2}

1. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `web_service_tunnel` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8449:10.241.0.5:8448 -J ubuntu@162.133.143.84 lsfadmin@10.241.16.6
    ```

    Since cluster-1 is already using port 8448 locally, you can map the Web Service to an alternative port (for example, 8449, 8450, etc.) for local access.
    {: note}

2. Open another terminal and run the command to copy the **cacert.pem** file from the management node to your local system:

    ```pre
    scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<Bastion_Node_IP> lsfadmin@<Management_Node_IP>:/opt/ibm/lsfsuite/ext/ws/conf/https/cacert pem /Users/test/Desktop/
    ```

    Note:

    * For clusters with a single management node, all configurations run on that same node. You can use the "ssh_to_management_node" tunnel for validation.

    * For clusters with multiple management nodes, Web Services are installed and configured on the second management node. In this case, replace <Management_Node_IP> with the IP address of management node 2.

3. Open another terminal and set up https certificate on Client or Localhost.

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf config set --cacert /Users/test/Desktop/cacert.pem
    OK
    test@abc-MacBook-Pro WebService_Certs % 
    ```

4. Log in to the LSF cluster using HTTPS on port 8449.

    ```pre
    test@abc-MacBook-Pro bin % ibmcloud lsf cluster logon --username lsfadmin --url https://localhost:8449
    Password>
    OK
    test@abc-MacBook-Pro bin %
    ```

    The default LSF user is lsfadmin. When prompted for a password, enter the Application Center password that was provided during the cluster creation.
    {: note}

After successful configuration, validate and run the workloads on different clusters from localhost.

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
    test@abc-MacBook-Pro ~ % lsf --cluster abc-jul10 lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
    My cluster name is abc-jul10
    My master name is abc-jul10-mgmt-1-2fe8-001.hpc.local
    test@abc-MacBook-Pro ~ %
    test@abc-MacBook-Pro ~ %
    test@abc-MacBook-Pro ~ % lsf --cluster abc-vnc lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
    My cluster name is abc-vnc
    My master name is abc-vnc-mgmt-1-8ab5-001.hpc.local
    test@abc-MacBook-Pro ~ %
    ```

3. Submit the workload to the specific cluster.

    ```pre
    test@abc-MacBook-Pro ~ % lsf --cluster abc-vnc bsub -n 1 sleep 1
    Job <102> is submitted to default queue <normal>.
    test@abc-MacBook-Pro ~ % lsf --cluster abc-vnc bjobs -a
    JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
    102     lsfadmi DONE  normal     abc-vnc-m abc-vnc-c sleep 1    Jul 16 07:10
    test@abc-MacBook-Pro ~ %
    ```

4. List all the jobs from the specific cluster.

    ```pre
    test@abc-MacBook-Pro ~ % lsf --cluster abc-jul10 bjobs -a
    No job found
    test@abc-MacBook-Pro ~ %
    ```

5. Log out of the cluster connection.

    ```pre
    test@MacBook-Pro ~ % lsf cluster logout --url https://localhost:8449
    lsfadmin is logged out.
    test@MacBook-Pro ~ %
    ```
