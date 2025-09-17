---

copyright:
  years: 2025
lastupdated: "2025-09-17"

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

# Small Medium Large Deployments
{: #sml-intro}

The current LSF setup is designed for production grade deployments. This approach is high-priced for trying before-you-buy option and demonstration use cases. As a solution, now users can select the deployment options using three different t-shirt sizes - Small, Medium, and Large. This solution has the ability to deploy a smaller and less expensive environment on IBM Cloud to try the capability or to provide a demonstration.

## Deployment types
{: #deploy-types}

You will be able to choose from these 3 deployment size options:

1. **Small (Minimal)**

    This deploys the smallest possible environment (a single management instance) for the fastest setup. All optional services like observability, logging, SCC, Atracker, and LDAP are disabled.

2. **Medium (Demo)**

    This displays the full set of capabilities. All optional services like observability, logging, and SCC are enabled. The deployment takes longer compared to minimal.

3. **Large (Production)**

    This option allows customization for production grade deployments. The optional services like observability, logging, and SCC are enabled by default but can be changed as required.

    All the JSON files are customizable (users can make configuration changes as needed). But the **.env** file is mandatory because it contains the required variables that must be filled always.

## Update the .env file
{: #env-file}
{: step}

The following commands are required to update the **.env** file.

```pre
##############################################################################
# Environment Configuration

# Step 1: Update the variables below as needed.
# Step 2: If you require additional optional variables, update them directly
#         in the JSON file(s) for your deployment type.
# Step 3: Always validate the JSON file before running the script.
##############################################################################

# IBM Cloud API key
API_KEY="YOUR_API_KEY"

# Account and resource details
ACCOUNT_GUID="ACCOUNT_GUID"
ZONES="ZONES"
RESOURCE_GROUP="RESOURCE_GROUP"

# SSH key name
SSH_KEY="SSH_KEY"

# Template JSON file (choose as per your deployment type)
TEMPLATE_FILE="catalog_values_minimal_deployment.json"

# LSF tile version locator
LSF_TILE_VERSION="1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.2ad06fe1-6125-45c5-b8b6-6454eb4907e6-global"

# App Center GUI password
# Rules: Minimum 8 characters, at least 1 uppercase, 1 lowercase, 1 number,
# and 1 special character (!@#$%^&*()_+=-). No spaces allowed.
APP_CENTER_GUI_PASSWORD="APP_CENTER_GUI_PASSWORD"
```

From the above snippet, below are the descriptions for the parameters:

* **API_KEY** - This key is used to authenticate your deployment and grant the necessary access to create and manage resources in your IBM Cloud environment.
* **ACCOUNT_GUID** - Login to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials. Go to **Manage** > **Account** > **Account settings**. You will find the Account ID for the resources.
* **ZONES** - The IBM Cloud zone within the chosen region where the IBM Spectrum LSF cluster will be deployed.
* **RESOURCE_GROUP** - The existing resource group in your IBM Cloud account where VPC resources will be deployed.
* **SSH_KEY** - A list of SSH key names are already configured in your IBM Cloud account to establish a connection to the Spectrum LSF nodes.
* **TEMPLATE_FILE** - All the .json files are uploaded in https://github.ibm.com/workload-eng-services/HPCaaS/tree/sml/tools/minimal-demo-prod-scripts
* **LSF_TILE_VERSION** - Login to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials. You can see the tile version in **Select a variation** box.
* **APP_CENTER_GUI_PASSWORD** - This is the password that is required to access the IBM Spectrum LSF Application Center (App Center) GUI, which is enabled by default in both Fix Pack 15 and Fix Pack 14 with HTTPS. This is a mandatory value and omitting it will result in deployment failure.

## Deploy the LSF environment
{: #deploy-env}
{: step}

Run the following commands to deploy the LSF environment:

```pre
1. chmod +x create_lsf_environment.sh
2. ./create_lsf_environment.sh <cluster_prefix>
```

* `create_lsf_environment` - This script automates the end-to-end deployment of an IBM Cloud LSF environment. It installs required plugins, generates configuration files from your **.env**, triggers the Schematics workspace deployment, and finally the prints access details (bastion, login, management IPs) with next steps for connecting and submitting jobs.

## Connect the LSF cluster and run the jobs
{: #connect-lsf-jobs}
{: step}

Now that your environment is set up, you can connect to the LSF cluster and perform operations such as submitting jobs, monitoring workloads, viewing infrastructure details.
There are two ways to connect:

### Option 1: Using Web Services
{: #using-web-services}

Run the `web_service.sh` to configure LSF web services for local client access (Standalone LSF Client):

```pre
chmod +x web_service.sh
./web_service.sh <cluster_prefix>
```

* `web_service.sh` - This script configures LSF Web Services for local client access (Standalone LSF Client). This will allow users to connect to an LSF cluster from their local environment.

    * Installs required IBM Cloud plugins and the LSF client package.
    * Configures environment values, logs in to IBM Cloud, and targets the right region.
    * Retrieves the Schematics workspace outputs to establish a secure web service tunnel to the cluster.
    * Copies the required TLS certificate, configures the client, and logs into the LSF cluster.
    * Verifies the cluster with `lsid` and `bhosts`.

    This allows you to connect to an LSF cluster from their local environment.

### Option 2: Using Utility Scripts
{: #using-utility-scripts}

1. Run the following command to view the infra details:
    ```pre
    chmod +x show.sh
    ./show.sh <cluster_prefix>
    ```

    * `show.sh` - This script retrieves details of the Schematics workspace for a given LSF cluster prefix. It ensures you are logged into the correct account and region, locates the workspace, and then displays its full configuration and state.

2. Copy the job submission script to the cluster by using the command:
    ```pre
    chmod +x cp.sh
    ./cp.sh <cluster_prefix> submit.sh
    ```

    * `cp.sh` - This script copies the `submit.sh` file into your LSF cluster. It validates account and region, fetches the bastion, login, and management IPs, and then securely transfers the `submit.sh` file either to the login node (default) or the management node (if management is specified).
    * `submit.sh` - This script demonstrates how to submit a sample job to the LSF scheduler. It provides a simple command (sleep 30) wrapped in an LSF job submission request (bsub). By default, it requests 8 CPU cores for the job.

    Customers can update:
    * Job options (for example, -n 8 to change the number of requested cores).
    * Command (for example, replace sleep 30 with their own workload).

    This serves as a template for testing job submission and can be adapted for real workloads.

3. Run the following command to jump to the LSF environment:
    ```pre
    chmod +x jump.sh
    ./jump.sh <cluster_prefix>
    ```

    * `jump.sh` - This script connects you directly to the LSF login node. It ensures you are targeting the right IBM Cloud account/region, fetches the bastion, login, and management IPs, and then uses SSH (with bastion as a jump host) to securely log into the LSF login node.

4. Run the following commands to submit the jobs:
    ```pre
    sh submit.sh
    bjobs
    lshosts -w
    ```

    * `submit.sh` - This script demonstrates how to submit a sample job to the LSF scheduler. It provides a simple command (sleep 30) wrapped in an LSF job submission request (bsub). By default, it requests 8 CPU cores for the job.
