---

copyright:
  years: 2025
lastupdated: "2025-06-12"

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

# Accessing REST API calls with `pacclient.py`
{: #access-rest-api-calls-pacclient}

{{site.data.keyword.spectrum_full_notm}} Application Center provides standard RESTful web services for application submission, data management, job information query, job actions, and more. The LSF Application Center web service API can be integrated with many languages and methods. This example shows how to access the LSF Application Center REST API calls by using [`pacclient.py`](https://www.ibm.com/docs/en/slac/10.2.0?topic=services-pacclientpy){: external}, which is a Python 3-based client.
{: shortdesc}

This example assumes that the LSF Application Center is configured to use the REST APIs through `https`. For more information, see [LSF Application Center Web Services](https://www.ibm.com/docs/en/slac/10.2.0?topic=lsf-application-center-web-services){: external}.
{: note}

## Before you begin
{: #before-you-begin}

Before you connect to the LSF Application Center with `https`, you need to complete the following steps:

1. To access the LSF Application Center REST APIs from your device, you need to set up an SSH connection. You can connect to an LSF management node through a new command line from your local device. The {{site.data.keyword.bpshort}} log output contains the LSF Application Center information that you need.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8443:10.241.0.10:8443 -L 6080:10.241.0.10:6080 -L 8444:10.241.0.10:8444 -J ubuntu@{bastion_node_ip} lsfadmin@{login_host_ip}
    ```

    where `login_host_ip` needs to be replaced with the login node IP address that is associated with `<cluster_prefix>-login-host-0`, and `FLOATING_IP_ADDRESS` needs to be replaced with the bastion node-floating IP address. To find the management and login node IPs, see the instructions for [Gathering IP addresses](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-accessing-lsf-gui&interface=ui#gathering-ip-addresses).

2. Open a second terminal, download, and install the Python 3.6 version or newer. The following examples are for Linux and MAC systems. If you are using Windows, the installation is different. For more information, see [Python releases for Windows](https://www.python.org/downloads/windows/){: external}.

    Run all the following commands from this second terminal:
    {: important}

    1. Install Python 3 (3.6 or newer) and `pip3` by running the following commands:

        ```pre
        yum install python3
        yum install python3-pip
        ```

    2. Install `httplib2` by running the following command:

        ```pre
        pip3 install httplib2
        ```

    3. Install `configparser` by running the following command:

        ```pre
        pip3 install configparser
        ```

    4. Install `urllib3` by running the following command:

        ```pre
        pip3 install urllib3
        ```

3. Clone the following repository to your local device:

    ```pre
    git clone https://github.com/IBMSpectrumComputing/lsf-integrations.git
    cd Spectrum\/LSF\/Application\/Center/pacclient/
    ```

4. On your local terminal, define or export the following variables:

    ```pre
    export SSL_INSECURE=1
    export AC_HOST=localhost
    export AC_PORT=8443
    export AC_USER=lsfadmin
    export AC_PASSWORD=<mynicepassword>
    ```

## Connecting to LSF Application Center with `https`
{: #connect-lsf-application-center-https}

Open a new command-line and run the following commands:

```python
$ ./pacclient.py help
 pacclient.py usage:

 ping      --- Check whether the web service is available
 logon     --- Log on to IBM Spectrum LSF Application Center
 logout    --- Log out from IBM Spectrum LSF Application Center
 app       --- List applications or parameters of an application
 submit    --- Submit a job
 job       --- Show information for one or more jobs
 jobaction --- Perform a job action on a job
 jobdata   --- List all the files for a job
 download  --- Download job data for a job
 upload    --- Upload job data for a job
 usercmd   --- Perform a user command
 useradd   --- Add a user to IBM Spectrum LSF Application Center
 userdel   --- Remove a user from IBM Spectrum LSF Application Center
 userupd   --- Updates user email in CSV format from IBM Spectrum LSF Application Center.
 pacinfo   --- Displays IBM Spectrum LSF Application Center version, build number and build date
 notification --- Displays the notification settings for the current user.
          --- Registers notifications for a workload.
 flow          --- Show details for one or more flow instances from IBM Spectrum LSF Process Manager.
 flowaction    --- Perform an action on a flow instance from IBM Spectrum LSF Process Manager.
 flowdef       --- Show details for one or more flow definitions from IBM Spectrum LSF Process Manager.
 flowdefaction --- Perform an action on a flow definition from IBM Spectrum LSF Process Manager.
 help      --- Display command usage

 # Login to the LSF Cluster.
$ ./pacclient.py logon -l https://$AC_HOST:$AC_PORT -u $AC_USER -p $AC_PASSWORD
You have logged on to PAC as: lsfadmin

 # Submit a Job in LSF cluster.
 $ ./pacclient.py submit -a generic -p "COMMANDTORUN=sleep 200"
 The job has been submitted successfully: job ID 45439

 # List out existing and running jobs.
 $ ./pacclient.py job
 JOBID     STATUS    EXTERNAL_STATUS        JOB_NAME                 COMMAND
 45439     Running   -                      *938772910               sleep 200

 # Stop running job.
 $ ./pacclient.py usercmd -c "bstop 45439"
 Job <45439> is being stopped

 $ ./pacclient.py job
 JOBID     STATUS    EXTERNAL_STATUS        JOB_NAME                 COMMAND
 45439     Suspended -                      *938772910               sleep 200

 # Resume running job.
 $ ./pacclient.py usercmd -c "bresume 45439"
 Job <45439> is being resumed

 $ ./pacclient.py job
 JOBID     STATUS    EXTERNAL_STATUS        JOB_NAME                 COMMAND
 45439     Running   -                      *938772910               sleep 200
```
{: codeblock}
