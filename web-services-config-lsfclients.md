---

copyright:
  years: 2025
lastupdated: "2025-09-22"

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

1. Download the LSF Client Package. Go to [IBM Fix Central](https://www.ibm.com/support/fixcentral/swg/downloadFixes?parent=IBM%20Spectrum%20Computing&product=ibm/Other+software/IBM+Spectrum+LSF&release=All&platform=All&function=fixId&fixids=lsf-10.1.0.15-spk-2025-Apr-build602430&includeRequisites=1&includeSupersedes=0&downloadMethod=http&login=true) and log in with your IBM credentials. Navigate to the IBM Spectrum LSF page, and download **lws_client10.1.0.15.tar.Z (65.45 MB)** file to your local machine (download may take ~10 minutes).

    If you want to download the package from CLI then right-click on "lws_client10.1.0.15.tar.Z (65.45 MB)" and copy the link. On client system, change into specific folder where you want to download and run below example command:

    ```pre
    wget https://delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0d0x0/5/Xa.2/Xb.jusyLTSp44S02bbew-D6h54MCqzdVNcCSkpQhgF62Al6ivcOhc_WQs48Q9E/Xc.CM/OS/0d0x0/5/lws_client10.1.0.15.tar.Z/Xd./Xf.Lpr./Xg.13527807/Xi.habanero/XY.habanero/XZ.o6spodGVxLohBHukLPwbszqW17NqggWP/lws_client10.1.0.15.tar.Z
    ```

2. Extract the downloaded file and select the folder for your operating system.

    * For "macOS": Install the Client Binary. Copy the lsf binary to your system path (for example, /usr/local/bin/) using:

    ```pre
    cp -pr lws_client10.1.0.15/mac-aarch64/lsf /usr/local/bin/
    ```

    * For Linux (x86_64): Install the client binary by copying it to your system path (for example, /usr/bin/) using:

    ```pre
    cp -pr lws_client10.1.0.15/x86_64/lsf /usr/bin/
    ```

3. Run the following command to check whether the LSF client is set up correctly:

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf version
    version:  v0.0.1  
    commit:   0.0.1  
    test@abc-MacBook-Pro WebService_Certs %
    ```

4. Delete the **.bluemix** folder to remove the outdated LSF plugin configs and credentials, a mandatory cleanup to ensure fresh tokens and avoid login or connectivity issues when switching clusters, updating the client, or fixing authentication errors.

    ```pre
    rm -rf "$HOME/.bluemix"
    ```

5. Copy the **cacert.pem** file from the management node to your local system using the command:

    ```pre
    scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<Bastion_Node_IP>lsfadmin@<Management_Node_IP>:/opt/ibm/lsfsuite/ext/ws/conf/https/cacert.pem /Users/test/Desktop/
    ```

    **Note:**

    * For clusters with a single management node, all the configurations run on that same node. You can use the "ssh_to_management_node" tunnel for validation.

    * For clusters with multiple management nodes, Web Services are installed and configured on the second management node. In this case, replace <Management_Node_IP> with the IP address of management node 2.

6. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `web_service_tunnel` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8448:localhost:8448 -J ubuntu@<Bastion_Node_IP> lsfadmin@<Management_Node_IP>
    ```

7. Open another terminal and set up the https certificate on the Client or Localhost.

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf config set --cacert
    /Users/test/Desktop/cacert.pem
    OK
    test@abc-MacBook-Pro WebService_Certs % 
    ```

8. Log in to the LSF cluster using HTTPS on port 8448. Port 8448 is the default secure port for LSF Web Services, ensuring all client-to-cluster communication is encrypted and isolated from other LSF services.


    ```pre
    test@abc-MacBook-Pro bin % lsf cluster logon --username lsfadmin --url https://localhost:8448
    Password>
    OK
    test@abc-MacBook-Pro bin %  
    ```

    The default LSF user is lsfadmin. When prompted for a password, enter the Application Center password that was provided during cluster creation.
    {: note}

9. Once the configuration is complete, you will be able to run any LSF commands from the same client node.

    ```pre
    test@abc-MacBook-Pro WebService_Certs % lsf lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.

    My cluster name is abc-re1
    My master name is abc-re1-mgmt-1-9e6e-001.hpc.local
    test@abc-MacBook-Pro WebService_Certs %

    test@abc-MacBook-Pro WebService_Certs % lsf bhosts -w
    HOST_NAME          STATUS          JL/U    MAX  NJOBS    RUN  SSUSP  USUSP    RSV
    abc-re1-comp-1-9e6e-001.hpc.local ok              -      8      0      0      0      0      0
    abc-re1-mgmt-1-9e6e-001.hpc.local closed_Full     -      0      0      0      0      0      0
    abc-re1-mgmt-1-9e6e-002.hpc.local closed_Full     -      0      0      0      0      0      0
    test@abc-MacBook-Pro WebService_Certs %
    test@abc-MacBook-Pro WebService_Certs % lsf bsub -n 5 sleep 10
    Job <228> is submitted to default queue <normal>.
    test@abc-MacBook-Pro WebService_Certs %
    test@abc-MacBook-Pro WebService_Certs % lsf bjobs -a -w
    JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
    227     lsfadmin DONE  normal     abc-re1-mgmt-1-9e6e-001.hpc.local abc-re1-comp-1-9e6e-001.hpc.local sleep 10   Jun 25 14:52
    228     lsfadmin DONE  normal     abc-re1-mgmt-1-9e6e-001.hpc.local 5*abc-re1-comp-1-9e6e-001.hpc.local sleep 10   Jun 25 15:04
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

1. Delete the **.bluemix** folder to remove the outdated LSF plugin configs and credentials, a mandatory cleanup to ensure fresh tokens and avoid login or connectivity issues when switching clusters, updating the client, or fixing authentication errors.

    ```pre
    rm -rf "$HOME/.bluemix"
    ```

2. Download the Client Package. Go to [IBM Fix Central](https://www.ibm.com/support/fixcentral/swg/downloadFixes?parent=IBM%20Spectrum%20Computing&product=ibm/Other+software/IBM+Spectrum+LSF&release=All&platform=All&function=fixId&fixids=lsf-10.1.0.15-spk-2025-Apr-build602430&includeRequisites=1&includeSupersedes=0&downloadMethod=http&login=true) and log in with your IBM credentials. Navigate to the IBM Spectrum LSF page, and download **lws_client10.1.0.15.tar.Z (65.45 MB)** file to your local machine (download may take ~10 minutes).

    If you want to download the package from CLI then right-click on "lws_client10.1.0.15.tar.Z (65.45 MB)" and copy the link. On client system, change into specific folder where you want to download and run below example command:

    ```pre
    wget https://delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0d0x0/5/Xa.2/Xb.jusyLTSp44S02bbew-D6h54MCqzdVNcCSkpQhgF62Al6ivcOhc_WQs48Q9E/Xc.CM/OS/0d0x0/5/lws_client10.1.0.15.tar.Z/Xd./Xf.Lpr./Xg.13527807/Xi.habanero/XY.habanero/XZ.o6spodGVxLohBHukLPwbszqW17NqggWP/lws_client10.1.0.15.tar.Z
    ```

3. Extract the downloaded file and select the folder for your operating system.

    * For "macOS": Install the Client Binary. Copy the lsf binary to your system path (for example, /usr/local/bin/) using:

    ```pre
    cp -pr lws_client10.1.0.15/mac-aarch64/lsf /usr/local/bin/
    ```

    * For Linux (x86_64): Install the client binary by copying it to your system path (for example, /usr/bin/) using:

    ```pre
    cp -pr lws_client10.1.0.15/x86_64/lsf /usr/bin/
    ```

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

8. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `web_service_tunnel` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8448:localhost:8448 -J ubuntu@<Bastion_Node_IP> lsfadmin@<Management_Node_IP>
    ```

9. Open another terminal and run the command to copy the **cacert.pem** file from the management node to your local system:

    ```pre
    scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<Bastion_Node_IP> lsfadmin@<Management_Node_IP>:/opt/ibm/lsfsuite/ext/ws/conf/https/cacert.pem /Users/test/Desktop/
    ```

    **Note:**

    * For clusters with a single management node, all configurations run on that same node. You can use the "ssh_to_management_node" tunnel for validation.

    * For clusters with multiple management nodes, Web Services are installed and configured on the second management node. In this case, replace <Management_Node_IP> with the IP address of management node 2.

10. Set up the https certificate on the client host using the command:

    ```pre
    test@MacBook-Pro WebService_Certs % ibmcloud lsf config set --cacert
    /Users/test/Desktop/lws_client10.1.0.15/cacert.pem
    OK
    test@MacBook-Pro WebService_Certs % 
    ```

11. Login to the LSF cluster using HTTPS on port 8448.

    ```pre
    test@abc-MacBook-Pro bin % ibmcloud lsf cluster logon --username lsfadmin --url https://localhost:8448
    Password>
    OK
    test@abc-MacBook-Pro bin %
    ```

    The default LSF user is lsfadmin. When prompted for a password, enter the Application Center password that was provided during cluster creation.
    {: note}

12. Once the configuration is complete, you will be able to run any LSF commands from the same client node.

    ```pre
    test@MacBook-Pro WebService_Certs % ibmcloud lsf lsid
    IBM Spectrum LSF 10.1.0.15, Apr 14 2025
    Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
    Copyright International Business Machines Corp. 1992, 2016.
    US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
    My cluster name is cluster_prefix-jul10
    My master name is cluster_prefix-jul10-mgmt-1-2fe8-001.hpc.local
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

## Accessing LSF Web Service through SSH Local Port Forwarding
{: #webservice-ssh-tunnel}

We need to securely access the LSF Web Service running on the management node from
our local system. Since the management node is not directly accessible from the
internet, you need to connect through the Bastion Node (jump host) using SSH local port
forwarding.

Run the following command from your local terminal:

```pre
ssh -L 8448:localhost:8448 -J ubuntu@<Bastion_Node> lsfadmin@<Management_Node>
```

How does the local port forwarding command works?

```pre
[ Local Machine:8448 ] --> [ Bastion Node ] --> [ Management Node:8448 ]
        ^                                                    |
        |                                                    |
Access via browser/CLI ----------- SSH Tunnel ---------------
```

Local Port Forwarding creates an encrypted SSH tunnel that maps a port on your local system to a service port on a remote host. This enables you to reach services that are not directly accessible (for example, those behind firewalls or only available through a Bastion/Jump host).

It allows you to access the LSF Web Service at: `https://localhost:8448`

Keep this terminal session open to maintain the tunnel; the connection will be lost
if the SSH tunnel times out or disconnects.

## Login and password policies
{: #password-complexity}

When connecting to the LSF Web Services client through SSH tunneling, ensure that the following prerequisites are met:

1. **Password Policy** – The password must:
    * Should be atleast 15 characters long.
    * Must include one lowercase letter, one uppercase letter, one number, and one special character (!@#$%^&*()_+=-).
    * Should not contain spaces.
      Only passwords meeting these criteria will be accepted for authentication.

2. **Do not use the password files** – For security reasons, storing passwords in plain text files or using command substitution (for example, --password "$(cat ~/.lsf_password)") is not supported. This exposes credentials to unnecessary risk.

3. **Failed login attempts** – Currently, the LSF Client does not enforce limits on incorrect password attempts over the tunnel. It is the responsibility of the cluster administrator to configure OS-level policies (such as PAM or faillock) to restrict failed login attempts and to maintain proper audit logging.

    Edit PAM configuration (for SSH logins):
    ```pre
    sudo vi /etc/pam.d/sshd
    ```

    Add these lines near the top (before pam_unix.so):

    ```pre
    auth required pam_faillock.so preauth silent deny=3 unlock_time=600
    auth [default=die] pam_faillock.so authfail deny=3 unlock_time=600
    auth sufficient pam_faillock.so authsucc
    ```

    * deny=3 → lock account after 3 failed attempts
    * unlock_time=600 → locked for 10 minutes (600s)

    Tell SSHD to use PAM (usually already enabled):
    Check **/etc/ssh/sshd_config:**
    ```pre
    UsePAM yes
    ```

    Restart services:
    ```pre
    sudo systemctl restart sshd
    ```

4. **Account lockout handling** – If the OS admin account gets locked after exceeding the maximum number of failed attempts, users can still access the cluster using the default OS account (vpcuser). From there, the lock can be cleared by the administrator for further login.
