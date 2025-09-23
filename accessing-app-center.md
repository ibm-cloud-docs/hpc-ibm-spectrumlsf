---

copyright:
  years: 2025
lastupdated: "2025-09-23"

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
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Accessing the application center
{: #accessing-gui}

The IBM Spectrum LSF Application Center is a web-based portal that provides an intuitive interface for working with IBM Spectrum LSF (Load Sharing Facility). It allows users to submit, monitor, and manage workloads on an LSF cluster without requiring command-line expertise.

Designed for end-users, developers, and administrators, the Application Center simplifies job submission and management, enabling a more efficient HPC experience. For more information, see [IBM Spectrum LSF Application Center](https://www.ibm.com/docs/en/slac/10.2.0?topic=lsf-application-center-web-services).

## Key Features
{: #key-features}

* **Web-based access** – Use a browser to submit and manage jobs, no LSF client installation required.

* **Application templates** – Simplified job submission with predefined templates for popular applications (for example, ANSYS, MATLAB, Abaqus).

* **Interactive Sessions** – Launch remote desktops or applications directly on compute nodes.

* **Job Monitoring and Control** – View job status, access logs, and cancel or resubmit jobs with a few clicks.

* **Data Management** – Upload, download, and organize files through the portal.

* **Role-based access** – Different views and permissions for administrators, power users, and standard users.

* **Cluster integration** – Works seamlessly with IBM Spectrum LSF resource scheduling and HPC environments.

## Configuration Overview
{: #config-overview}

The Application Center is installed and enabled by default with the IBM Spectrum LSF Suite. By default, it runs on the secondary management node (Management Node 2). If your cluster has only one management node, the service runs on that node.
The service runs as a single instance. If the node is down, the Application Center will be temporarily unavailable.

High Availability (HA) is not enabled by default.
{: note}

## Accessing the LSF Application Center GUI
{: #access-lsf-gui}

1. Open an SSH tunnel.

    From your local terminal, connect to the LSF management node using the SSH tunnel details provided in your deployment logs (look for the variable `application_center_tunnel`).

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8443:localhost:8443 -L 6080:localhost:6080 -L 8444:localhost:8444 -J ubuntu@<Bastion_Node_IP> lsfadmin@<Management_Node_IP>
    ```

2. Manage the Application Center service.

    On the management node, you can manage the Application Center service with the following commands:

    * Check status:

    ```pre
    pmcadmin list
    ```

    * Stop the service:

    ```pre
    pmcadmin stop
    ```

    * Start the service:

    ```pre
    pmcadmin start
    ```

3. Launch the GUI.

a. Open a web browser on your local system. Recommendation: Use Safari for the best experience.

b. Navigate to `https://localhost:8443`

c. Log in with the following credentials:

* Username: lsfadmin
* Password: The Application Center password provided during cluster creation.
