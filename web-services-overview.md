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

# About IBM Spectrum LSF Web Services
{: #about-web-services}

IBM Spectrum LSF Web Services provides a RESTful HTTP(s) interface for remotely interacting with LSF clusters. This helps users to programmatically submit, monitor, and manage jobs, enabling seamless integration of LSF workload management into custom applications, web portals, and automation workflows. Spectrum LSF Web Services is introduced in Fix Pack 15. This eliminates the need for direct command-line access and offers secure, scalable access through standard APIs.

## Configuring IBM Spectrum LSF Web Services
{: #config-web-services}

IBM Spectrum LSF Web Services is enabled by default with the LSF Suite installation. The service is installed and runs on the primary management node (usually Management Node 1).

By default, High Availability (HA) for Web Services is not enabled. The service runs as a stand-alone instance on a single node. If the node becomes unavailable, then the Web Services will also be temporarily inaccessible.
{: note}

## Checking the IBM Spectrum LSF Web Services
{: #checking-lsf-web-services}

1. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the `connect_to_web_services` variable.

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8448:localhost:8448 -J ubuntu@165.192.135.237 lsfadmin@10.241.0.4
    ```

2. Following are the commands to manage the Web Services:

* To check the status of the Web Service - `systemctl status lwsd`

* Start the Web Service - `systemctl start lwsd`

* Stop the Web Service - `systemctl stop lwsd`
