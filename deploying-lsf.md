---

copyright:
  years: 2025
lastupdated: "2025-06-26"

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:table: .aria-labeledby="caption"}

# Deploying {{site.data.keyword.spectrum_full_notm}}
{: #deploy-architecture}

Deploy the {{site.data.keyword.spectrum_full_notm}} deployable architecture with Spectrum LSF cluster using either the {{site.data.keyword.cloud_notm}} console UI, or the {{site.data.keyword.cloud_notm}} catalog CLI, and then access the deployed environment.
{: shortdesc}

## Creating the project by using the UI
{: #deploy-project-gui}
{: ui}

You can deploy your {{site.data.keyword.spectrum_full}} cluster by using the {{site.data.keyword.cloud_notm}} console UI to create an {{site.data.keyword.spectrum_full_notm}} project.

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
2. In the _Works with_ section, select **LSF** and then select the {{site.data.keyword.spectrum_full_notm}} tile.
3. In the _Variation_ section, select **Cluster with LSF v10.1.0.14** to indicate {{site.data.keyword.spectrum_full_notm}} and version as the scheduler to use for the deployment.
4. Select the **Product version** (in **Architecture**) that you want to install.
5. Click **Review deployment options**.
6. In the _Deployment options_ section, click **Add to project** to add this deployment to an {{site.data.keyword.cloud_notm}} project and combine it with other deployments, if applicable. {{site.data.keyword.cloud_notm}} projects include several more pipeline steps before deployment, including deployment validation, cost calculation, compliance verification, and approval process.
7. In the _Create a project_ section:
    * Specify a **Name** for your {{site.data.keyword.spectrum_full}} project.
    * Optionally provide a **Description** to describe the purpose of the project.
    * Specify a **Configuration name** for your {{site.data.keyword.spectrum_full}} project. The name can be up to 64 characters.
    * Select a **Region** for the location where you want the {{site.data.keyword.spectrum_full}} project deployed. The region for the LSF project container can be different from the actual region where the cluster is deployed.
    * Select a **Resource group** for where to get resources for your {{site.data.keyword.spectrum_full}} project.

    Click **Add** to save and add your project. When created, the project is added to the **Projects** view of the {{site.data.keyword.cloud_notm}} console.

8. In the _Configure_ section of the _Edit configuration_ page, edit the configuration by entering the **Security**, **Required**, and **Optional** input values.

    Descriptions of the deployment input values are next to each variable in the {{site.data.keyword.cloud_notm}} console.
    {: tip}

    Secure deployment input values might be entered directly or might be referenced from an existing [{{site.data.keyword.cloud}} Secrets Manager](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). As a best practice, the more secure option is to use a Secrets Manager to store secured input values.

    * In the **Security** tab, you have two sections:
        * **Authentication**: specify an API key for the {{site.data.keyword.cloud_notm}} account where you want to deploy your {{site.data.keyword.spectrum_full}} cluster to fulfill the `ibmcloud_api_key` input variable.
        * **Security and compliance**: configure the {{site.data.keyword.compliance_full}} controls that you want to use to validate the deployable architecture code before the deployment. You can use the architecture defaults or select your own from an existing {{site.data.keyword.compliance_short}} instance. When you deploy the {{site.data.keyword.spectrum_full}} cluster and create a new {{site.data.keyword.compliance_short}} instance, you set these deployment input variables in the **Optional** tab.

    * In the **Required** tab, specify the deployment values for the mandatory input variables: `app_center_gui_password`, `existing_resource_group`, `ibmcloud_api_key`, `lsf_version`, `remote_allowed_ips`, `ssh_keys`, and `zones`.

    For production clusters, work with your business owners or license management team to make sure that your organization has procured enough licenses to deploy the LSF cluster by using {{site.data.keyword.spectrum_full_notm}}.

    * In the **Optional** tab, specify deployment values for advanced configuration and for deeper customization of the provisioned elements.

    For example, to enable the `observability_monitoring_enable` variable, you need to set the value to **true**.

    Click **Save** to save your configuration options.

9. Click **Validate**.
   {{site.data.keyword.cloud_notm}} projects run a Code Risk Analyzer scan that includes a [supported set of {{site.data.keyword.compliance_short}} rules](/docs/ContinuousDelivery?topic=ContinuousDelivery-cra-cli-plugin#terraform-scc-goals). It checks controls that are part of the {{site.data.keyword.spectrum_full}} deployment and that {{site.data.keyword.cloud_notm}} projects support. Any extra controls that are not included in the list of supported {{site.data.keyword.compliance_short}} rules are not checked when you validate the configuration.

   Provide a comment to approve the validation and proceed to deployment.

10. Click **Deploy** to proceed with the deployment. Deploying the deployable architecture can take several minutes. You are notified when the deployment is successful. Optionally click **View resources** from the **Summary** tab to see details about the deployed {{site.data.keyword.spectrum_full}} project.

When deployed, you can then access your deployed environment.

## Deploying {{site.data.keyword.spectrum_full_notm}} by using the CLI
{: #create-project-cli}
{: cli}

To generate the API key, refer [Managing user API keys](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=cli).
{: note}

To login to the IBM Cloud CLI, refer [ibmcloud login](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).
{: note}

You can deploy your {{site.data.keyword.spectrum_full}} cluster by using the {{site.data.keyword.cloud_notm}} CLI to create a catalog workspace with the supported {{site.data.keyword.spectrum_full}} version. The CLI requires a `values.json` file with your configuration settings.

1. Install the [{{site.data.keyword.cloud_notm}} CLI and the catalogs management plug-in](https://cloud.ibm.com/docs/cli?topic=cli-manage-catalogs-plugin) before you run any CLI commands.

2. The CLI requires a `version_locator_value`. You can retrieve this value from the {{site.data.keyword.cloud_notm}} console UI:

    1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
    2. In the _Works with_ section, select **LSF** and then select the {{site.data.keyword.spectrum_full_notm}} tile.
    2. In the _Variation_ section, select **Cluster with LSF v10.1.0.14** to indicate {{site.data.keyword.spectrum_full_notm}} and version as the scheduler to use for the deployment.
    4. Select the **Product version** (in **Architecture**) that you want to install.
    5. Click **Review deployment options**.
    6. In the _Deployment options_ section, select **Create from the CLI**, copy the `version_locator_value`, and save this value to be used in a later step. The value is an 80 character alphanumeric string, such as:

        ```text
        1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.398b4df2-8186-4326-b31d-d8a5af20d8fc-global
        ```
        {: codeblock}

2. The CLI requires a `values.json` file with your configuration settings. Use the following example `values.json` file as a reference: you can copy the contents, change the values to meet your own deployment configurations, and then save it as `values.json`.

    Take note of these mandatory deployment input values:
    * Provide the mandatory deployment values for your {{site.data.keyword.spectrum_full}} cluster, specifically, replace the **Fill here** text with values for `app_center_gui_password`, `existing_resource_group`, `ibmcloud_api_key`, `lsf_version`, `remote_allowed_ips`, `ssh_keys`, and `zones`.

3. Run this command in the {{site.data.keyword.cloud_notm}} CLI to deploy your {{site.data.keyword.spectrum_full}} cluster with the configuration you specified in your `values.json` file; you use your `version_locator_value` copied and saved in a previous step, here:

    ```text
    ibmcloud catalog install --vl <version_locator_value> --override-values values.json
    ```
    {: codeblock}

    For example, running:

    ```text
    ibmcloud catalog install --vl 1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.398b4df2-8186-4326-b31d-d8a5af20d8fc-global --override-values values.json
    ```
    {: codeblock}

    outputs:

    ```text
    Attempting install of {{site.data.keyword.spectrum_full_notm}} version x.x.x...
    Schematics workspace: https://cloud.ibm.com/schematics/workspaces/us-south.workspace.globalcatalog-collection.40b1c1e4/jobs?region=
    Workspace status: DRAFT
    Workspace status: INACTIVE
    Workspace status: INPROGRESS
    Workspace status: ACTIVE
    Installation successful
    OK
    ```
    {: codeblock}

When deployed, you can then access your deployed environment.

## Accessing the deployed environment
{: #access-deployed-environment}

Regardless of whether you deployed the {{site.data.keyword.spectrum_full}} environment by using the {{site.data.keyword.cloud_notm}} console UI or the CLI after you deploy:

* Verify that you have access to the bastion host by using an SSH key.
* Verify that you can log in to all created {{site.data.keyword.spectrum_full_notm}} instances.
* Verify that you can connect to the {{site.data.keyword.spectrum_full}} environment by using the following SSH commands:

Run the command to login to the management node:

```ssh
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<bastion_node_IP> lsfadmin@<management_node_IP>
```
{: codeblock}


Run the command for the login node:

```ssh
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<bastion_node_IP> lsfadmin@<login_node_ip>
```
{: codeblock}

For example:

```ssh
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@150.239.215.145 lsfadmin@10.241.0.4
```
{: codeblock}


If you deployed by using a project, you can copy this SSH command from the {{site.data.keyword.cloud_notm}} console: select **Projects > _project_name_ > Configurations > _project_configuration_name_ > Outputs** tab, and use the copy icon to copy the `ssh_command` value and run it from a command line.
{: tip}
