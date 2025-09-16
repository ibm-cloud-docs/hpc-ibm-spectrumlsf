---

copyright:
  years: 2025
lastupdated: "2025-09-16"

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

The current LSF deployment setup is designed for production and is expensive for trying before-you-buy option and demonstration use cases. As a solution, now users can select the deployment options using three different t-shirt sizes. This solution has the ability to deploy a smaller and less expensive environment on IBM Cloud to try the capability or to provide a demonstration.

## Deployment types
{: #deploy-types}

There are 3 deployment size options, you will be able to choose from:

1. **Minimal**

    This deploys the smallest possible environment (a single management instance) for the fastest setup. All optional services like observability, logging, SCC, Atracker, and LDAP are disabled.

2. **Demo**

    This displays the full set of capabilities. All optional services like observability, logging, and SCC are enabled. The deployment takes longer compared to minimal.

3. **Production**

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

# SSH key name (must exist in your account)
SSH_KEY="SSH_KEY"

# Template JSON file (choose as per your deployment type)
TEMPLATE_FILE="catalog_values_minimal_deployment.json"

# LSF tile version locator
# Example below is for 3.0.1 version
LSF_TILE_VERSION="1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.2ad06fe1-6125-45c5-b8b6-6454eb4907e6-global"

# App Center GUI password
# Rules: Minimum 8 characters, at least 1 uppercase, 1 lowercase, 1 number,
# and 1 special character (!@#$%^&*()_+=-). No spaces allowed.
APP_CENTER_GUI_PASSWORD="APP_CENTER_GUI_PASSWORD"
```

## Deploy the LSF environment
{: #deploy-env}
{: step}

Run the following commands to deploy the LSF environment:

```pre
1. chmod +x create_lsf_environment.sh
2. ./create_lsf_environment.sh <cluster_prefix>
```

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

This allows you to connect to an LSF cluster from their local environment.

### Option 2: Using Utility Scripts
{: #using-utility-scripts}

1. Run the following command to view the infra details:
    ```pre
    chmod +x show.sh
    ./show.sh <cluster_prefix>
    ```

2. Copy the job submission script to the cluster by using the command:
    ```pre
    chmod +x cp.sh
    ./cp.sh <cluster_prefix> submit.sh
    ```

3. Run the following command to jump to the LSF environment:
    ```pre
    chmod +x jump.sh
    ./jump.sh <cluster_prefix>
    ```

4. Run the following commands to submit the jobs:
    ```pre
    sh submit.sh
    bjobs
    lshosts -w
    ```
