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

# Configuring LSF Web Services with clients
{: #configure-web-service}

The LSF client is the primary interface that allows users to connect to an LSF cluster from their local environment. It enables users to submit jobs, monitor job status, and efficiently manage workloads across distributed computing resources.

There are two types of LSF clients:

1. Standalone LSF Client (lsf)
2. IBM Cloud LSF Plugin (ibmcloud lsf)

## Standalone LSF Client (lsf)
{: #standalone-lsf-client}

The Standalone LSF Client is a traditional command-line tool installed locally and is used to connect directly to an on-premise or remote LSF cluster.

1. Download the `lws_client10.1.0.15.tar.Z` package from the [IBM Fix Central](https://www.ibm.com/support/fixcentral/swg/downloadFixes?parent=IBM%20Spectrum%20Computing&product=ibm/Other+software/IBM+Spectrum+LSF&release=All&platform=All&function=fixId&fixids=lsf-10.1.0.15-spk-2025-Apr-build602430&includeRequisites=1&includeSupersedes=0&downloadMethod=http&login=true).

2. Extract the zip folder and choose the operating system to set up the `lws client`. Choose the binary and copy into "/usr/local/bin/" path.
    Example: `cp -pr lws_client10.1.0.15/mac-aarch64/lsf /usr/local/bin/`

3. Run the following command to check whether the LSF client is set up correctly:

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf version
    version:  v0.0.1  
    commit:   0.0.1  
    test@abc-MacBook-Pro WebService_Certs %
    ```

4. If the .bluemix folder exists, then delete the LSF plugin directory by removing `$HOME/.bluemix/plugins/lsf` using the command:
    `rm -rf "$HOME/.bluemix"`

5. Copy the `cacert.pem` file from the management node to your local system using the command:

    ```pre
    scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J
    ubuntu@165.192.143.29
    lsfadmin@10.241.0.4:/opt/ibm/lsfsuite/ext/ws/conf/https/cacert.pem
    /Users/test/Desktop/
    ```

6. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `connect_to_web_services` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8448:localhost:8448 -J ubuntu@165.192.135.237 lsfadmin@10.241.0.4
    ```

7. Open another terminal and set up the https certificate on the Client or Localhost.

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf config set --cacert
    /Users/test/Desktop/cacert.pem
    OK
    test@abc-MacBook-Pro WebService_Certs % 
    ```

8. Login to the LSF cluster using HTTPS on port 8448.

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf cluster logon --username
    lsfadmin --password lsf@123 --url https://localhost:8448
    OK
    test@abc-MacBook-Pro WebService_Certs % 
    ```

    Here "lsfadmin" is the default LSF user and “AppCenter” is the UI password.
    {: note}

9. Once the configuration is complete, you will be able to run any LSF commands from the same client node.

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.

    My cluster name is abc-re1
    My master name is abc-re1-mgmt-1-9e6e-001.lsf.com
    test@abc-MacBook-Pro WebService_Certs %

    test@abc-MacBook-Pro WebService_Certs % lsf bhosts -w
    HOST_NAME          STATUS          JL/U    MAX  NJOBS    RUN  SSUSP  USUSP    RSV
    abc-re1-comp-1-9e6e-001.lsf.com ok              -      8      0      0      0      0      0
    abc-re1-mgmt-1-9e6e-001.lsf.com closed_Full     -      0      0      0      0      0      0
    abc-re1-mgmt-1-9e6e-002.lsf.com closed_Full     -      0      0      0      0      0      0
    test@abc-MacBook-Pro WebService_Certs %
    test@abc-MacBook-Pro WebService_Certs % lsf bsub -n 5 sleep 10
    Job <228> is submitted to default queue <normal>.
    test@abc-MacBook-Pro WebService_Certs %
    test@abc-MacBook-Pro WebService_Certs % lsf bjobs -a -w
    JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
    227     lsfadmin DONE  normal     abc-re1-mgmt-1-9e6e-001.lsf.com abc-re1-comp-1-9e6e-001.lsf.com sleep 10   Jun 25 14:52
    228     lsfadmin DONE  normal     abc-re1-mgmt-1-9e6e-001.lsf.com 5*abc-re1-comp-1-9e6e-001.lsf.com sleep 10   Jun 25 15:04
    test@abc-MacBook-Pro WebService_Certs %
    test@abc-MacBook-Pro WebService_Certs %
    ```

10. Log out of the cluster connection.

    ```pre
    test@MacBook-Pro ~ % lsf cluster logout --url https://localhost:8448
    lsfadmin is logged out.
    test@MacBook-Pro ~ % 
    ```

## IBM Cloud LSF Plugin (ibmcloud lsf)
{: #lsf-plugin}

The IBM Cloud LSF Plugin is a cloud-native plugin for the IBM Cloud CLI that allows users to interact with LSF clusters deployed in IBM Cloud environments.

1. Remove the `$HOME/.bluemix/plugins/lsf` if it exists from your client or local system using the command - `rm –fr  ~/.bluemix`

2. Download the `lws_client10.1.0.15.tar.Z` package from the [IBM Fix Central](https://www.ibm.com/support/fixcentral/swg/downloadFixes?parent=IBM%20Spectrum%20Computing&product=ibm/Other+software/IBM+Spectrum+LSF&release=All&platform=All&function=fixId&fixids=lsf-10.1.0.15-spk-2025-Apr-build602430&includeRequisites=1&includeSupersedes=0&downloadMethod=http&login=true).

3. Extract the zip folder and choose the operating system to set up the `lws client`. Choose the binary and copy into "/usr/local/bin/" path.
    `cp -pr lws_client10.1.0.15/mac-aarch64/lsf /usr/local/bin/`

4. Install the IBM Cloud CLI on your client or local system:
    `curl -fsSL https://clis.cloud.ibm.com/install/linux | sh`

5. Change the directory to “/usr/local/bin/”
    `cd /usr/local/bin/`

6. Install the LSF plugin.

    ```pre
    test@MacBook-Pro bin % ibmcloud plugin install lsf   
    Installing binary...
    OK
    Plug-in 'lsf 0.0.1' was successfully installed into /Users/test/.bluemix/plugins/lsf. Use 'ibmcloud plugin show lsf' to show its details.
    test@MacBook-Pro bin %
    ```

7. Verify the IBM Cloud LSF client.

    ```pre
    test@MacBook-Pro bin % ibmcloud lsf               
    NAME:
    lsf, lsf - LSF web service client.
    USAGE:
    ibmcloud lsf [global_options...] command [arguments...] [options...]
    GLOBAL OPTIONS:
    --cluster value  Set a target LSF cluster by its name
    Alias: -c value
    --env Submit job with user's local environment variables
    Alias: -e
    COMMANDS:
    cluster, cl    Manage LSF clusters.
    config, conf   Manage configuration.
    file, f        Manage LSF File.
    help, h        Show help.
    version, v     Display the 'lsf' command-line interface version.
    $LSFCOMMAND    Execute LSF CLI command.
    The available LSF commands are:

    bacct, bapp, bbot, bchkpnt, bconf, bentags, bgadd, bgdel, bgmod, bgpinfo, bhist, bhosts, bhpart, bjdepinfo, bjgroup, bjobs, bkill, blaunch, blimits, bmgroup, bmig, bmod, bparams, bpeek, bpost, bqueues, bread, brequeue, bresize, bresources, brestart, bresume, brlainfo, brsvadd, brsvdel, brsvmod, brsvs, brun, bsla, bslots, bstatus, bstop, bsub, bswitch, btop, bugroup, busers, lsacct, lsacctmrg, lsclusters, lseligible, lsrun, lsgrun, lshosts, lsid, lsinfo, lsload, lsloadadj, lsltasks, lsplace, lsrtasks, qdel, qsub

    Type `ibmcloud lsf help` in the terminal to get more details on any of the above commands.
    ```

8. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `connect_to_web_services` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8448:localhost:8448 -J ubuntu@165.192.135.237 lsfadmin@10.241.0.4
    ```

9. Open another terminal and run the command to copy the `cacert.pem` file from the management node to your local system:

    ```pre
    scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J
    ubuntu@165.192.143.29
    lsfadmin@10.241.0.4:/opt/ibm/lsfsuite/ext/ws/conf/https/cacert.pem
    /Users/test/Desktop/
    ```

10. Set up the https certificate on the client host using the command:

    ```pre
    test@MacBook-Pro WebService_Certs % ibmcloud lsf config set --cacert
    /Users/test/Desktop/lws_client10.1.0.15/cacert.pem
    OK
    test@MacBook-Pro WebService_Certs % 
    ```

11. Login to the LSF cluster using HTTPS on port 8448.

    ```pre
    test@MacBook-Pro WebService_Certs % ibmcloud lsf cluster logon --username lsfadmin --password abc@123 --url https://localhost:8448
    OK
    test@MacBook-Pro WebService_Certs %
    ```

    Here "lsfadmin" is the default LSF user and “AppCenter” is the UI password.
    {: note}

12. Once the configuration is complete, you will be able to run any LSF commands from the same client node.

    ```pre
    test@MacBook-Pro WebService_Certs % ibmcloud lsf lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
    My cluster name is cluster_prefix-jul10
    My master name is cluster_prefix-jul10-mgmt-1-2fe8-001.lsf.com
    test@MacBook-Pro WebService_Certs % 
    test@MacBook-Pro WebService_Certs % 
    test@MacBook-Pro WebService_Certs % ibmcloud lsf lshosts
    HOST_NAME      type    model  cpuf ncpus maxmem maxswp server RESOURCES
    cluster_prefix-jul10  X86_64 Intel_E5  12.5     8  62.3G      -    Yes (mg)
    cluster_prefix-jul10  X86_64 Intel_E5  12.5     -      -      -     No ()
    test@MacBook-Pro WebService_Certs % ibmcloud lsf bhosts 
    HOST_NAME          STATUS       JL/U    MAX  NJOBS    RUN  SSUSP  USUSP    RSV 
    cluster_prefix-jul10-mgmt-1 closed          -      0      0      0      0      0      0
    test@MacBook-Pro WebService_Certs % ibmcloud lsf bjobs -a
    No job found
    test@MacBook-Pro WebService_Certs % 
    ```

12. Log out of the cluster connection.

    ```pre
    test@MacBook-Pro WebService_Certs % ibmcloud lsf cluster logout
    --url https://localhost:8448
    lsfadmin is logged out.
    test@MacBook-Pro WebService_Certs % 
    ```
