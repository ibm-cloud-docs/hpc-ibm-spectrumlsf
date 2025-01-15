---

copyright:
  years: 2025
lastupdated: "2025-01-15"

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

You can deploy your {{site.data.keyword.spectrum_full_notm}} cluster by using the {{site.data.keyword.cloud_notm}} console UI to create an {{site.data.keyword.spectrum_full_notm}} project.

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
2. In the _Works with_ section, select **LSF** and then select the {{site.data.keyword.spectrum_full_notm}} tile.
3. In the _Variation_ section, select **Cluster with LSF v10.1.0.14** to indicate {{site.data.keyword.spectrum_full_notm}} and version as the scheduler to use for the deployment.
4. Select the **Product version** (in **Architecture**) that you want to install.
5. Click **Review deployment options**.
6. In the _Deployment options_ section, click **Add to project** to add this deployment to an {{site.data.keyword.cloud_notm}} project and combine it with other deployments, if applicable. {{site.data.keyword.cloud_notm}} projects include several more pipeline steps before deployment, including deployment validation, cost calculation, compliance verification, and approval process.
7. In the _Create a project_ section:
    * Specify a **Name** for your {{site.data.keyword.spectrum_full_notm}} project.
    * Optionally provide a **Description** to describe the purpose of the project.
    * Specify a **Configuration name** for your {{site.data.keyword.spectrum_full_notm}} project. The name can be up to 64 characters.
    * Select a **Region** for the location where you want the {{site.data.keyword.spectrum_full_notm}} project deployed. The region for the LSF project container can be different from the actual region where the cluster is deployed.
    * Select a **Resource group** for where to get resources for your {{site.data.keyword.spectrum_full_notm}} project.

    Click **Add** to save and add your project. When created, the project is added to the **Projects** view of the {{site.data.keyword.cloud_notm}} console.

8. In the _Configure_ section of the _Edit configuration_ page, edit the configuration by entering the **Security**, **Required**, and **Optional** input values.

    Descriptions of the deployment input values are next to each variable in the {{site.data.keyword.cloud_notm}} console.
    {: tip}

    Secure deployment input values might be entered directly or might be referenced from an existing [{{site.data.keyword.cloud}} Secrets Manager](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). As a best practice, the more secure option is to use a Secrets Manager to store secured input values.

    * In the **Security** tab, you have two sections:
        * **Authentication**: specify an API key for the {{site.data.keyword.cloud_notm}} account where you want to deploy your {{site.data.keyword.spectrum_full_notm}} cluster to fulfill the `ibmcloud_api_key` input variable.
        * **Security and compliance**: configure the {{site.data.keyword.compliance_full}} controls that you want to use to validate the deployable architecture code before the deployment. You can use the architecture defaults or select your own from an existing {{site.data.keyword.compliance_short}} instance. When you deploy the {{site.data.keyword.spectrum_full_notm}} cluster and create a new {{site.data.keyword.compliance_short}} instance, you set these deployment input variables in the **Optional** tab.

    * In the **Required** tab, specify the deployment values for the mandatory input variables: `cluster_id`, `cluster_prefix`, `reservation_id`, `remote_allowed_ips`, `resource_group`, `ssh_key_name`, `ibm_customer_number` and `zone`.

    For production clusters, work with your business owners or license management team to make sure that your organization has procured enough licenses to deploy the LSF cluster by using {{site.data.keyword.spectrum_full_notm}}.

    You can specify single availability zones, in the same supported IBM Cloud [region](/docs/allowlist/hpc-service?topic=hpc-service-ha-dr), for your Spectrum LSF cluster nodes. For example, zones `us-east-1` and `us-east-3` in the **us-east** region, and zones `eu-de-2` and `eu-de-3` in the **eu-de** region.

    * In the **Optional** tab, specify deployment values for advanced configuration and for deeper customization of the provisioned elements.

        Also, take note of these optional deployment input values:
        * Specify between [two methods for accessing the bastion node in the cluster](/docs/allowlist/hpc-service?topic=hpc-service-before-you-begin-deploying&interface=ui#select-method-for-accessing-cluster): directly through a floating IP that is attached to the bastion node (`enable_fip`, **true** by default), or through a VPN gateway (`vpn_enabled`, **false** by default). Regardless of which access method you select, values for `remote_allowed_ips` must be provided to identify a list of IP addresses of systems that can access the bastion node.

        * To use a [custom image for compute nodes](/docs/allowlist/hpc-service?topic=hpc-service-custom-image), provide the CRN for that image for the `compute_image_name` value.

        * You use {{site.data.keyword.filestorage_vpc_full_notm}} for file sharing. By default, you can use two file share volumes; each is 100 GB. To change this configuration, [set the `custom_file_shares` deployment value](/docs/allowlist/hpc-service?topic=hpc-service-ibm-cloud-hpc-faqs#share).

        * To [integrate your {{site.data.keyword.scale_full}} cluster with {{site.data.keyword.spectrum_full_notm}}](/docs/allowlist/hpc-service?topic=hpc-service-integrating-scale), use the `cluster_subnet_id`, `custom_file_shares`, and `storeage_security_group_id` deployment values.

        * To create DNS zones, provide [an existing {{site.data.keyword.cloud}} DNS Service instance ID](/docs/dns-svcs?topic=dns-svcs-getting-started) for the `dns_instance_id` deployment input value. Alternatively, if you leave the `dns_instance_id` deployment input value as null, the deployment process creates a new DNS service instance ID and the respective DNS zone.

        * To create DNS custom resolvers, and you have an existing VPC, provide the resolver ID for the `dns_custom_resolver_id` deployment input value. Alternatively, if you leave the `dns_custom_resolver_id` deployment input value as null, the deployment process creates a new VPC, and a creates and enables a new custom resolver for your cluster.

        * To enable a [flow log collector for your VPC](/docs/vpc?topic=vpc-flow-logs), set `enable_vpc_flow_logs` to **true**, and provide values for `create_authorization_policy_vpc_to_cos`, `existing_cos_instance_guid`, `existing_storage_bucket_name`, and `is_flow_log_collector_active`.

        * To enable monitoring metrics for your {{site.data.keyword.spectrum_full_notm}} cluster using {{site.data.keyword.monitoringfull_notm}}, [enable the monitoring settings](/docs/allowlist/hpc-service?topic=hpc-service-monitoring) using the `observability_monitoring_enable`, `observability_monitoring_on_compute_nodes_enable`, and `observability_monitoring_plan` input variables.

        * To use [customer-managed encryption](/docs/allowlist/hpc-service?topic=hpc-service-before-you-begin-deploying&interface=ui#encryption), specify the encryption input variables: `enable_customer_managed_encryption`, `kms_instance_id`, and `kms_key_name`.

        * To create an {{site.data.keyword.compliance_short}} instance that checks to your environment for security issues and validates the deployable architecture code during {{site.data.keyword.spectrum_full_notm}} cluster deployment, configure the `scc_enable`, `scc_location`, `scc_profile`, and `scc_profile_version` input variables.

            The {{site.data.keyword.compliance_short}} instance addition does not represent an infrastructure cost. Its billing is based on its evaluations. For more information about {{site.data.keyword.compliance_short}} pricing, see the [{{site.data.keyword.compliance_short}} documentation](/docs/security-compliance?topic=security-compliance-scc-pricing).

            Note that if you have an existing {{site.data.keyword.compliance_short}} instance to validate the deployable architecture code, these deployment input variables are in the **Security** deployment tab.

        * To configure and use [{{site.data.keyword.spectrum_full_notm}} Application Center](/docs/allowlist/hpc-service?topic=hpc-service-about-application-center) to submit and monitor LSF jobs for your {{site.data.keyword.spectrum_full_notm}} cluster from a GUI interface, set `enable_app_center` to **true**, and `app_center_gui_pwd` to match your LSF Application Center password (which must be at least 8 characters, contain one number, one lowercase letter, one uppercase letter, and at least one special character (for example, **Admin@123**)). (For more information about detailed LSF Application Center usage, see [{{site.data.keyword.spectrum_full_notm}} Application Center product documentation](https://www.ibm.com/docs/en/slac/10.2.0){: external}.)

            By default, LSF Application Center high availability is enabled (that is, the `app_center_high_availability` input variable is set to **true** by default). To fully configure high availability, also complete the [predeployment steps for LSF Application Center high availability](/docs/allowlist/hpc-service?topic=hpc-service-before-deploy-application-center).

        * To use [OpenLDAP with your {{site.data.keyword.spectrum_full_notm}} cluster](/docs/allowlist/hpc-service?topic=hpc-service-integrate-openldap-spectrum-lsf)  for centralized user management, robust security, and simplified user authentication, configure the `enable_ldap`, `ldap_basedns`, `ldap_server`,`ldap_server_cert`, `ldap_admin_password`, `ldap_user_name`, and `ldap_user_password` input values.

    Click **Save** to save your configuration options.
9. Click **Validate**.
   {{site.data.keyword.cloud_notm}} projects run a Code Risk Analyzer scan that includes a [supported set of {{site.data.keyword.compliance_short}} rules](/docs/code-risk-analyzer-cli-plugin?topic=code-risk-analyzer-cli-plugin-cra-cli-plugin#terraform-scc-goals). It checks controls that are part of the {{site.data.keyword.spectrum_full_notm}} deployment and that {{site.data.keyword.cloud_notm}} projects support. Any extra controls that are not included in the list of supported {{site.data.keyword.compliance_short}} rules are not checked when you validate the configuration.

   Provide a comment to approve the validation and proceed to deployment.
10. Click **Deploy** to proceed with the deployment. Deploying the deployable architecture can take several minutes. You are notified when the deployment is successful. Optionally click **View resources** from the **Summary** tab to see details about the deployed {{site.data.keyword.spectrum_full_notm}} project.

When deployed, you can then access your deployed environment.

## Deploying {{site.data.keyword.spectrum_full_notm}} by using the CLI
{: #create-project-cli}
{: cli}

You can deploy your {{site.data.keyword.spectrum_full_notm}} cluster by using the {{site.data.keyword.cloud_notm}} CLI to create a catalog workspace with the supported {{site.data.keyword.spectrum_full_notm}} version. The CLI requires a `values.json` file with your configuration settings.

1. Install the [{{site.data.keyword.cloud_notm}} CLI and the catalogs management plug-in](https://cloud.ibm.com/docs/cli?topic=cli-manage-catalogs-plugin) before you run any CLI commands.

2. The CLI requires a `version_locator_value`. You can retrieve this value from the {{site.data.keyword.cloud_notm}} console UI:

    1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
    2. In the _Works with_ section, select **LSF** and then select the {{site.data.keyword.spectrum_full_notm}} tile.
    2. In the _Variation_ section, select **Cluster with LSF v10.1.0.14** to indicate {{site.data.keyword.spectrum_full_notm}} and version as the scheduler to use for the deployment.
    4. Select the **Product version** (in **Architecture**) that you want to install.
    5. Click **Review deployment options**.
    6. In the _Deployment options_ section, select **Create from the CLI**, copy the `version_locator_value`, and save this value to be used in a later step. The value is an 80 character alphanumeric string, such as:

        ```text
        1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.c7645085-5f49-4d5f-8786-45ac376e60fe-global
        ```
        {: codeblock}

2. The CLI requires a `values.json` file with your configuration settings. Use the following example `values.json` file as a reference: you can copy the contents, change the values to meet your own deployment configurations, and then save it as `values.json`.

    Take note of these mandatory deployment input values:
    * Provide the mandatory deployment values for your {{site.data.keyword.spectrum_full_notm}} cluster, specifically, replace the **Fill here** text with values for `ibmcloud_api_key`, `cluster_id`, `cluster_prefix`, `reservation_id`, `remote_allowed_ips`, `resource_group`, `ssh_key_name`, and `zone`.

    * Your capacity reservation ID and cluster ID are provided by {{site.data.keyword.IBM}} technical sales. Before you deploy, verify that you have these IDs so that you can input them as `cluster_id` and `reservation_id` input values when you deploy the {{site.data.keyword.spectrum_full_notm}} environment.

    * You can specify two availability zones, in the same [region](/docs/allowlist/hpc-service?topic=hpc-service-ha-dr), for your {{site.data.keyword.cloud_notm}} HPC cluster nodes. For example, zones `us-east-1` and `us-east-3` in the **us-east** region, and zones `eu-de-2` and `eu-de-3` in the **eu-de** region.

        By default, the first zone from the variable `var.zone` is where the management nodes and compute nodes are to be created; the second zone is where the dynamic compute nodes are created for cross-zone support.

    Also, take note of these optional deployment input values:
    * Specify between [two methods for accessing the bastion node in the cluster](/docs/allowlist/hpc-service?topic=hpc-service-before-you-begin-deploying&interface=ui#select-method-for-accessing-cluster): directly through a floating IP that is attached to the bastion node (`enable_fip`, **false** by default), or through a VPN gateway (`vpn_enabled`, **false** by default). Regardless of which access method you select, values for `remote_allowed_ips` must be provided to identify a list of IP addresses of systems that can access the bastion node.

    * To use a [custom image for compute nodes](/docs/allowlist/hpc-service?topic=hpc-service-custom-image),  provide the CRN for that image for the `compute_image_name` value.

    * You use {{site.data.keyword.filestorage_vpc_full_notm}} for file sharing. By default, you can use two file share volumes; each is 100 GB. To change this configuration, [set the `custom_file_shares` deployment value](/docs/allowlist/hpc-service?topic=hpc-service-ibm-cloud-hpc-faqs#share).

    * To [integrate your {{site.data.keyword.scale_full}} cluster with {{site.data.keyword.spectrum_full_notm}}](/docs/allowlist/hpc-service?topic=hpc-service-integrating-scale), use the `cluster_subnet_id`, `custom_file_shares`, and `storeage_security_group_id` deployment values.

    * To create DNS zones, provide [an existing {{site.data.keyword.cloud}} DNS Service instance ID](/docs/dns-svcs?topic=dns-svcs-getting-started) for the `dns_instance_id` deployment input value. Alternatively, if you leave the `dns_instance_id` deployment input value as null, the deployment process creates a new DNS service instance ID and the respective DNS zone.

    * To create DNS custom resolvers, and you have an existing VPC, provide the resolver ID for the `dns_custom_resolver_id` deployment input value. Alternatively, if you leave the `dns_custom_resolver_id` deployment input value as null, the deployment process creates a new VPC, and a creates and enables a new custom resolver for your cluster.

    * To enable a [flow log collector for your VPC](/docs/vpc?topic=vpc-flow-logs), set `enable_vpc_flow_logs` to **true**, and provide values for `create_authorization_policy_vpc_to_cos`, `existing_cos_instance_guid`, `existing_storage_bucket_name`, and `is_flow_log_collector_active`.

    * To enable moritoring metrics for your {{site.data.keyword.spectrum_full_notm}} cluster using {{site.data.keyword.monitoringfull_notm}}, [enable the monitoring settings](/docs/allowlist/hpc-service?topic=hpc-service-monitoring) using the `observability_monitoring_enable`, `observability_monitoring_on_compute_nodes_enable`, and `observability_monitoring_plan` input variables.

    * To use [customer-managed encryption](/docs/allowlist/hpc-service?topic=hpc-service-before-you-begin-deploying&interface=ui#encryption), specify the encryption input variables: `enable_customer_managed_encryption`, `kms_instance_id`, and `kms_key_name`.

    * To create an {{site.data.keyword.compliance_full}} instance that checks to your environment for security issues and validates the deployable architecture code during {{site.data.keyword.spectrum_full_notm}} cluster deployment, configure the `scc_enable`, `scc_location`, `scc_profile`, and `scc_profile_version` input variables.

        The {{site.data.keyword.compliance_short}} instance addition does not represent an infrastructure cost. Its billing is based on its evaluations. For more information about {{site.data.keyword.compliance_short}} pricing, see the [{{site.data.keyword.compliance_short}} documentation](/docs/security-compliance?topic=security-compliance-scc-pricing).

    * To configure and use [{{site.data.keyword.spectrum_full_notm}} Application Center](/docs/allowlist/hpc-service?topic=hpc-service-about-application-center) to submit and monitor LSF jobs for your {{site.data.keyword.spectrum_full_notm}} cluster from a GUI interface, set `enable_app_center` to **true**, and `app_center_gui_pwd` to match your LSF Application Center password (which must be at least 8 characters, contain one number, one lowercase letter, one uppercase letter, and at least one special character (for example, **Admin@123**)). (For more information about detailed LSF Application Center usage, see [{{site.data.keyword.spectrum_full_notm}} Application Center product documentation](https://www.ibm.com/docs/en/slac/10.2.0){: external}.)

        By default, LSF Application Center high availability is enabled (that is, the `app_center_high_availability` input variable is set to **true** by default). To fully configure high availability, also complete the [predeployment steps for LSF Application Center high availability](/docs/allowlist/hpc-service?topic=hpc-service-before-deploy-application-center).

    * To use [OpenLDAP with your {{site.data.keyword.spectrum_full_notm}} cluster](/docs/allowlist/hpc-service?topic=hpc-service-integrate-openldap-spectrum-lsf)  for centralized user management, robust security, and simplified user authentication, configure the `enable_ldap`, `ldap_basedns`, `ldap_server`,`ldap_server_cert`, `ldap_admin_password`, `ldap_user_name`, and `ldap_user_password` input values.

3. Run this command in the {{site.data.keyword.cloud_notm}} CLI to deploy your {{site.data.keyword.spectrum_full_notm}} cluster with the configuration you specified in your `values.json` file; you use your `version_locator_value` copied and saved in a previous step, here:

    ```text
    ibmcloud catalog install --vl <version_locator_value> --override-values values.json
    ```
    {: codeblock}

    For example, running:

    ```text
    ibmcloud catalog install --vl 1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.c7645085-5f49-4d5f-8786-45ac376e60fe-global --override-values values.json
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

Regardless of whether you deployed the {{site.data.keyword.spectrum_full_notm}} environment by using the {{site.data.keyword.cloud_notm}} console UI or the CLI after you deploy:

* Verify that you have access to the bastion host by using an SSH key.
* Verify that you can log in to all created IBM Spectrum LSF instances.
* Verify that you can connect to the {{site.data.keyword.spectrum_full_notm}} environment by using the following SSH commands:

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
