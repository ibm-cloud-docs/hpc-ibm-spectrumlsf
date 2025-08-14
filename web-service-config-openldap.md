---

copyright:
  years: 2025
lastupdated: "2025-08-14"

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

# Configuring the LSF client using OpenLDAP
{: #config-openldap}

To configure Web Services client with OpenLDAP users, enable LDAP support in a new LSF cluster or integrate your existing LDAP server with the LSF cluster by following the [Using OpenLDAP with the deployed environment](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-about-openldap) document.

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

## Configuring LSF client with LDAP user and password
{: #config-lsf-client}

1. Use the following command to configure the LSF client with LDAP user and password:

    ```pre
    test@MacBook-Pro Cluster1 % lsf cluster logon --username test --password Admin@123 --url https://localhost:8448
    OK
    test@MacBook-Pro Cluster1 %
    ```

    Here, `test` is the LDAP user and "Admin@123" is the user password.
    {: note}

2. Once the LDAP user is configured, run the workloads like the `lsfadmin` user.

    ```pre
    test@MacBook-Pro ~ % lsf --cluster abc-vnc bsub -n 2 sleep 20

    Job <103> is submitted to default queue <normal>.

    test@MacBook-Pro ~ %

    test@MacBook-Pro ~ % lsf --cluster abc-vnc bjobs -a          

    JOBID   USER    STAT  QUEUE    FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME

    103     test    RUN    normal   abc-vnc-m   2*abc-vnc   sleep 20    Jul 16 08:07

    test@MacBook-Pro ~ % 

    test@MacBook-Pro ~ % lsf --cluster abc-vnc bjobs -a

    JOBID   USER    STAT    QUEUE   FROM_HOST    EXEC_HOST   JOB_NAME   SUBMIT_TIME

    103     test     DONE   normal  abc-vnc-m     2*abc-vnc   sleep 20   Jul 16 08:07

    test@MacBook-Pro ~ %
    ```
