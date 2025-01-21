---

copyright:
  years: 2025
lastupdated: "2025-01-21"

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
{:table: .aria-labeledby="caption"}

# Deployment values
{: #deployment-values}

The following deployment values can be used to configure the {{site.data.keyword.spectrum_short}} cluster instance on {{site.data.keyword.cloud}}:

NOT_SET refers to an empty value in the {{site.data.keyword.cloud_notm}} catalog tile. If you are using other modes like CLI, API, or Schematics UI directly to provision the HPC cluster, leave the values empty.
{: note}

| Value | Description | Is it required? | Default value |
| ----- | ----------- | --------------- | ------------ |
| `enable_app_center` | Set to true to enable the IBM Spectrum LSF Application Center GUI (default: false). [System requirements](https://www.ibm.com/docs/en/slac/10.2.0?topic=requirements-system-102-fix-pack-14) for IBM Spectrum LSF Application Center Version 10.2 Fix Pack 14. | No | false |
| `existing_certificate_instance` | When app_center_high_availability is enable/set as true, The Application Center is configured for high availability and requires an Application Load Balancer Front-End listener to use a certificate CRN value stored in the Secret Manager. Provide the valid 'existing_certificate_instance' to configure the Application load balancer. | No | "" |
| `app_center_gui_pwd` | Password for IBM Spectrum LSF Application Center GUI. Note: Password should be at least 8 characters, must have one number, one lowercase letter, one uppercase letter, and at least one special character. | No | "" |
| `app_center_high_availability` | Set to false to disable the IBM Spectrum LSF Application Center GUI High Availability (default: true). If the value is set as true, provide a certificate instance crn under the existing_certificate_instance value for the VPC load balancer to enable HTTPS connections [certificate instance requirements](https://cloud.ibm.com/docs/allowlist/hpc-service?topic=hpc-service-before-deploy-application-center). | No | true |
| `bastion_ssh_keys` | Provide the list of SSH key names configured in your IBM Cloud account to establish a connection to the IBM Cloud HPC bastion and login node. Ensure that the SSH key is present in the same resource group and region where the cluster is being provisioned. If you do not have an SSH key in your IBM Cloud account, create one by following the provided instructions [SSH Keys](https://cloud.ibm.com/docs/vpc?topic=vpc-ssh-keys). | Yes | None |
| `bastion_instance_name` | Provide the name of the bastion instance. If none is given then a new bastion will be created. | No | "" |
| `bastion_instance_public_ip` | Provide the public IP address of the bastion instance to establish the remote connection. | No | "" |
| `bastion_security_group_id` | Provide the security group ID of the bastion server. This security group ID is added as an allowlist rule on the HPC cluster nodes to establish an SSH connection through the bastion node. | No | "" |
| `bastion_ssh_private_key` | Provide the private SSH key (named id_rsa) used during the creation and configuration of the bastion server to securely authenticate and connect to the bastion server. This allows access to internal network resources from a secure entry point. Note: The corresponding public SSH key (named id_rsa.pub) must already be available in the ~/.ssh/authorized_keys file on the bastion host to establish authentication. | No | "" |
| `cluster_prefix` | Prefix that is used to name the IBM Cloud HPC cluster and IBM Cloud resources that are provisioned to build the IBM Cloud HPC cluster instance. You cannot create more than one instance of the IBM Cloud HPC cluster with the same name. Ensure that the name is unique. Prefix must start with a lowercase letter and contain only lowercase letters, digits, and hyphens in between. Hyphens must be followed by at least one lowercase letter or digit. There are no leading, trailing, or consecutive hyphens. Character length for cluster_prefix should be less than 16. | No | "hpcaas" |
| `cluster_id` | Ensure that you have received the cluster ID from IBM technical sales. A unique identifier for HPC cluster used by IBM Cloud HPC to differentiate different HPC clusters within the same reservations. This can be up to 39 alphanumeric characters including the underscore (_), the hyphen (-), and the period (.) characters. You cannot change the cluster ID after deployment. | Yes | None |
| `cluster_subnet_ids` | Provide the list of existing subnet ID under the existing VPC where the cluster will be provisioned. One subnet ID is required as the input value. Supported zones are: eu-de-2 and eu-de-3 for eu-de, us-east-1 and us-east-3 for us-east, and us-south-1 for us-south. The management nodes file storage shares, and compute nodes are deployed in the same zone. | No | [] |
| `compute_ssh_keys` | Provide the list of SSH key names configured in your IBM Cloud account to establish a connection to the IBM Cloud HPC cluster node. Ensure that the SSH key is present in the same resource group and region where the cluster is being provisioned. If you do not have an SSH key in your IBM Cloud account, create one by following the provided instructions [SSH Keys](https://cloud.ibm.com/docs/vpc?topic=vpc-ssh-keys). | Yes | None |
| `compute_image_name` | Name of the custom image that you want to use to create virtual server instances in your IBM Cloud account to deploy the IBM Cloud HPC cluster dynamic compute nodes. By default, the solution uses a RHEL 8-8 base OS image with additional software packages mentioned [here](https://cloud.ibm.com/docs/ibm-spectrum-lsf#create-custom-image). The solution also offers, Ubuntu 22-04 OS base image (hpcaas-lsf10-ubuntu2204-compute-v8). If you would like to include your application-specific binary files, follow the instructions in [ Planning for custom images ](https://cloud.ibm.com/docs/vpc?topic=vpc-planning-custom-images) to create your own custom image and use that to build the IBM Cloud HPC cluster through this offering. | No | "hpcaas-lsf10-rhel810-compute-v8" |
| `custom_file_shares` | Provide details for customizing your shared file storage layout, including mount points, sizes in GB, and IOPS ranges for up to five file shares. Each file share size in GB supports a different IOPS range. If the cluster requires creating more than 256 dynamic nodes, only provide the details of the NFS share and use \"/mnt/lsf\" as the mount path for the internal file share. If not, a default VPC file share is created, which supports up to 256 nodes. For more information, see [file share IOPS value](https://cloud.ibm.com/docs/vpc?topic=vpc-file-storage-profiles&interface=ui). | No | [{ mount_path = "/mnt/vpcstorage/tools", size = 100, iops = 2000 }, { mount_path = "/mnt/vpcstorage/data", size = 100, iops = 6000 }, { mount_path = "/mnt/scale/tools", nfs_share = "" }] |
| `cos_instance_name` | Provide the name of the existing Cloud Object Storage instance where the logs for the enabled functions will be stored. | No | "" |
| `dns_instance_id` | Provide the ID of an existing IBM Cloud DNS services domain to skip creating a new DNS service instance name.Note: If dns_instance_id is not equal to null, a new dns zone is created under the existing dns service instance. | No | "" |
| `dns_domain_name` | IBM Cloud DNS Services domain name to be used for the IBM Cloud HPC cluster. | No | {compute = "hpcaas.com"} |
| `dns_custom_resolver_id` | Provide the ID of an existing IBM Cloud DNS custom resolver to skip creating a new custom resolver. If the value is set to null, a new dns custom resolver shall be created and associated to the VPC. Note: A VPC can be associated only to a single custom resolver. Provide the ID of the custom resolver if it is already associated to the VPC. | No | "" |
| `enable_cos_integration` | Set to true to create an extra cos bucket to integrate with HPC cluster deployment. | No | false |
| `enable_vpc_flow_logs` | Flag to enable VPC flow logs. If true, a flow log collector is created. | No | false |
| `enable_fip` | The solution supports multiple ways to connect to your IBM Cloud HPC cluster, for example, by using a login node, or by using VPN or direct connection. If connecting to the IBM Cloud HPC cluster by using VPN or direct connection, set this value to false. | No | true |
| `enable_ldap` | Set this option to true to enable LDAP for IBM Cloud HPC, with the default value set to false. | No | false |
| `hyperthreading_enabled` | Setting this to true will enable hyper-threading in the compute nodes of the cluster (default). Otherwise, hyper-threading will be disabled. | No | true |
| `ibmcloud_api_key` | IBM Cloud API key for the IBM Cloud account where the IBM Cloud HPC cluster needs to be deployed. For more information on how to create an API key, see [Managing user API keys](https://cloud.ibm.com/docs/account?topic=account-userapikey). | Yes | None |
| `ibm_customer_number` | Comma-separated list of one or more IBM Customer Numbers (ICN) that is used for the Bring Your Own License (BYOL) entitlement check. For more information on how to find your ICN, see [What is my IBM Customer Number (ICN)?](https://www.ibm.com/support/pages/what-my-ibm-customer-number-icn). | No | "" |
| `key_management` | Set the value as key_protect to enable customer-managed encryption for boot volume and file share. If the key_management is set as null, IBM Cloud resources are will be always be encrypted through provider managed. | No | "key_protect" |
| `kms_instance_name` | Provide the name of the existing Key Protect instance associated with the Key Management Service. Note: To use existing kms_instance_name set key_management as key_protect. The name can be found under the details of the KMS, see [View key-protect ID](https://cloud.ibm.com/docs/key-protect?topic=key-protect-retrieve-instance-ID&interface=ui). | No | "" |
| `kms_key_name` | Provide the existing kms key name that you want to use for the IBM Cloud HPC cluster. Note: kms_key_name is considered only if key_management value is set as key_protect.(for example kms_key_name: my-encryption-key). | No | "" |
| `login_subnet_id` | Provide the list of existing subnet ID under the existing VPC, where the login/bastion server is provisioned. One subnet ID is required as an input value for the creation of login node and bastion in the same zone as the management nodes. Note: Provide a different subnet ID for login_subnet_id, do not overlap or provide the same subnet ID that was already provided for cluster_subnet_ids. | No | "" |
| `login_node_instance_type` | Specify the virtual server instance profile type to be used to create the login node for the IBM Cloud HPC cluster. For choices on profile types, see [Instance profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles). | No | "bx2-2x8" |
| `login_image_name` | Name of the custom image that you want to use to create virtual server instances in your IBM Cloud account to deploy the IBM Cloud HPC cluster login node. By default, the solution uses an RHEL 8-8 OS image with additional software packages mentioned [here](https://cloud.ibm.com/docs/ibm-spectrum-lsf#create-custom-image). The solution also offers, Ubuntu 22-04 OS base image (hpcaas-lsf10-ubuntu2204-compute-v7). If you would like to include your application-specific binary files, follow the instructions in [ Planning for custom images ](https://cloud.ibm.com/docs/vpc?topic=vpc-planning-custom-images) to create your own custom image and use that to build the IBM Cloud HPC cluster through this offering. | No | "hpcaas-lsf10-rhel810-compute-v8" |
| `ldap_basedns` | The dns domain name is used for configuring the LDAP server. If an LDAP server is already in existence, ensure to provide the associated DNS domain name. | No | "hpcaas.com" |
| `ldap_server` | Provide the IP address for the existing LDAP server. If no address is given, a new LDAP server is created. | No | "" |
| `ldap_server_cert` | Provide the existing LDAP server certificate. This value is required if the 'ldap_server' variable is not set to null. If the certificate is not provided or is invalid, the LDAP configuration may fail. For more information on how to create or obtain the certificate, refer to an [existing LDAP server certificate](https://cloud.ibm.com/docs/allowlist/hpc-service?topic=hpc-service-integrating-openldap). | No | "" |
| `ldap_admin_password` | The LDAP administrative password should be 8 to 20 characters long, with a mix of at least three alphabetic characters, including one uppercase and one lowercase letter. It must also include two numerical digits and at least one special character from (~@_+:) are required. It is important to avoid including the username in the password for enhanced security. This value is ignored for an existing LDAP server]. | No | "" |
| `ldap_user_name` | Custom LDAP User for performing cluster operations. Note: Username should be between 4 to 32 characters, (any combination of lowercase and uppercase letters).[   This value is ignored for an existing LDAP server]. | No | "" |
| `ldap_user_password` | The LDAP user password should be 8 to 20 characters long, with a mix of at least three alphabetic characters, including one uppercase and one lowercase letter. It must also include two numerical digits and at least one special character from (~@_+:) are required. It is important to avoid including the username in the password for enhanced security. This value is ignored for an existing LDAP server]. | No | "" |
| `ldap_vsi_profile` | Specify the virtual server instance profile type to be used to create the ldap node for the IBM Cloud HPC cluster. For choices on profile types, see [Instance profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles). | No | "cx2-2x4" |
| `ldap_vsi_osimage_name` | Image name to be used for provisioning the LDAP instances. By default the ldap server is created on the Ubuntu-based OS flavor. | No | "ibm-ubuntu-22-04-4-minimal-amd64-3" |
| `management_image_name` | Name of the custom image that you want to use to create virtual server instances in your IBM Cloud account to deploy the IBM Cloud HPC cluster management nodes. By default, the solution uses an RHEL88 base image with additional software packages mentioned [here](https://cloud.ibm.com/docs/ibm-spectrum-lsf#create-custom-image). If you would like to include your application-specific binary files, follow the instructions in [ Planning for custom images ](https://cloud.ibm.com/docs/vpc?topic=vpc-planning-custom-images) to create your own custom image and use that to build the IBM Cloud HPC cluster through this offering. | No | "hpcaas-lsf10-rhel810-v12" |
| `management_node_instance_type` | Specify the virtual server instance profile type to be used to create the management nodes for the IBM Cloud HPC cluster. For choices on profile types, see [Instance profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles). | No | "bx2-16x64" |
| `management_node_count` | Number of management nodes. This is the total number of management nodes. Enter a value between 1 and 10. | No | 3 |
| `observability_atracker_enable` | Activity Tracker Event Routing to configure how to route auditing events. While multiple Activity Tracker instances can be created, only one tracker is needed to capture all events. Creating additional trackers is unnecessary if an existing Activity Tracker is already integrated with a Cloud Object Storage bucket. In such cases, set the value to false, as all events can be monitored and accessed through the existing Activity Tracker. | No | true |
| `observability_atracker_target_type` | All the events are stored in either Cloud Object Storage bucket or Cloud Logs based on user input, so customers can retrieve or ingest them in their system. | No | "cloudlogs" |
| `observability_logs_enable_for_management` | Set false to disable IBM Cloud Logs integration. If enabled, infrastructure and LSF application logs from Management Nodes will be ingested. | No | false |
| `observability_logs_enable_for_compute` | Set false to disable IBM Cloud Logs integration. If enabled, infrastructure and LSF application logs from Compute Nodes will be ingested. | No | false |
| `observability_enable_platform_logs` | Setting this to true will create a tenant in the same region that the Cloud Logs instance is provisioned to enable platform logs for that region. NOTE: You can only have 1 tenant per region in an account. | No | false |
| `observability_enable_platform_metrics` | Receive platform metrics in the provisioned IBM Cloud Monitoring instance. | No | false |
| `observability_logs_retention_period` | The number of days IBM Cloud Logs retains the log data in Priority insights. Allowed values: 7, 14, 30, 60, 90. | No | 7 |
| `observability_monitoring_on_compute_nodes_enable` | Set false to disable IBM Cloud Monitoring integration. If enabled, infrastructure metrics from Compute Nodes will be ingested. | No | false |
| `observability_monitoring_plan` | Type of service plan for IBM Cloud Monitoring instance. You can choose one of the following: lite, graduated-tier. For all details visit [IBM Cloud Monitoring Service Plans](https://cloud.ibm.com/docs/monitoring?topic=monitoring-service_plans). | No | "graduated-tier" |
| `resource_group` | Specify the existing resource group name from your IBM Cloud account where the VPC resources should be deployed. By default, the resource group name is set to 'Default.' In some older accounts, the resource group name may be 'default,' so validate the resource_group name before deployment. If the resource group value is set to the string \"null\", the automation creates two different resource groups named 'workload-rg' and 'servicer.' For more information on resource groups, refer to Managing resource groups. | No | None |
| `reservation_id` | Ensure that you have received the reservation ID from IBM technical sales. Reservation ID is a unique identifier to distinguish different IBM Cloud HPC service agreements. It must start with a letter and can only contain letters, numbers, hyphens (-), or underscores (_). | No | "" |
| `remote_allowed_ips` | Comma-separated list of IP addresses that can access the IBM Cloud HPC cluster instance through an SSH interface. For security purposes, provide the public IP addresses assigned to the devices that are authorized to establish SSH connections (for example, [\"169.45.117.34\"]). To fetch the IP address of the device, use [https://ipv4.icanhazip.com/](https://ipv4.icanhazip.com/). | Yes | None |
| `solution` | Provide the value for the solution that is needed for the support of LSF and HPC. | No | lsf |
| `storage_security_group_id` | Provide the storage security group ID created from the Spectrum Scale storage cluster if the nfs_share value is updated to use the scale file set mount points under the cluster_file_share variable. | No | "" |
| `scc_enable` | Flag to enable SCC instance creation. If true, an instance of SCC (Security and Compliance Center) will be created. | No | false |
| `scc_profile` | Profile to be set on the SCC Instance (accepting empty, 'CIS IBM Cloud Foundations Benchmark' and 'IBM Cloud Framework for Financial Services'). | No | "CIS IBM Cloud Foundations Benchmark" |
| `scc_profile_version` | Version of the Profile to be set on the SCC Instance (accepting empty, CIS, and Financial Services profiles versions). | No | "1.0.0" |
| `scc_location` | Location where the SCC instance is provisioned (possible choices are 'us-south', 'eu-de', 'castor', 'eu-es'). | No | "us-south" |
| `scc_event_notification_plan` | Event notifications Instance plan to be used (it's used with S.C.C. instance), possible values 'lite' and 'standard'. | No | "lite" |
| `skip_iam_authorization_policy` | Set to false if authorization policy is required for VPC block storage volumes to access kms. This can be set to true if authorization policy exists. For more information on how to create authorization policy manually, see [creating authorization policies for block storage volume](https://cloud.ibm.com/docs/vpc?topic=vpc-block-s2s-auth&interface=ui). | No | false |
| `skip_iam_share_authorization_policy` | Set it to false if authorization policy is required for VPC file share to access kms. This can be set to true if authorization policy exists. For more information on how to create authorization policy manually, see [creating authorization policies for VPC file share](https://cloud.ibm.com/docs/vpc?topic=vpc-file-s2s-auth&interface=ui). | No | false |
| `skip_flowlogs_s2s_auth_policy` | Skip auth policy between flow logs service and Cloud Object Storage instance, set to true if this policy is already in place on account. | No | false |
| `TF_VERSION` | The version of the Terraform engine that's used in the Schematics workspace. | No | "1.5" |
| `TF_PARALLELISM` | Parallelism/ concurrent operations limit. Valid values are between 1 and 256, both inclusive. [Learn more](https://www.terraform.io/docs/internals/graph.html#walking-the-graph). | No | "250" |
| `TF_VALIDATION_SCRIPT_FILES` | List of script file names used by validation test suites. If provided, these scripts are executed as part of validation test suites execution. | No | [] |
| `vpc_name` | Name of an existing VPC in which the cluster resources will be deployed. If no value is given, then a new VPC will be provisioned for the cluster. [Learn more](https://cloud.ibm.com/docs/vpc). | No | "" |
| `vpc_cidr` | Creates the address prefix for the new VPC, when the vpc_name variable is empty. The VPC requires an address prefix for creation of subnet in a single zone. The subnet is created with the specified CIDR blocks. For more information, see [Setting IP ranges](https://cloud.ibm.com/docs/vpc?topic=vpc-vpc-addressing-plan-design). | No | "10.241.0.0/18" |
| `vpc_cluster_private_subnets_cidr_blocks` | Provide the CIDR block required for the creation of the compute cluster's private subnet. One CIDR block is required. If using a hybrid environment, modify the CIDR block to avoid conflicts with any on-premises CIDR blocks. Ensure that the selected CIDR block size can accommodate the maximum number of management and dynamic compute nodes that are expected in your cluster. For more information on CIDR block size selection, refer to the documentation. See [Choosing IP ranges for your VPC](https://cloud.ibm.com/docs/vpc?topic=vpc-choosing-ip-ranges-for-your-vpc). | No | ["10.241.0.0/20"] |
| `vpc_cluster_login_private_subnets_cidr_blocks` | Provide the CIDR block required for the creation of the login cluster's private subnet. Only one CIDR block is needed. If using a hybrid environment, modify the CIDR block to avoid conflicts with any on-premises CIDR blocks. Since the login subnet is used only for the creation of login virtual server instances, provide a CIDR range of /28. | No | ["10.241.16.0/28"] |
| `vpn_enabled` | Set the value as true to deploy a VPN gateway for VPC in the cluster. | No | false |
| `worker_node_min_count` | The minimum number of worker nodes. This is the number of static worker nodes that will be provisioned at the time the cluster is created. If using NFS storage, enter a value in the range 0 - 500. If using Spectrum Scale storage, enter a value in the range 1 - 64. NOTE: Spectrum Scale requires a minimum of 3 compute nodes (combination of management-host, management-host-candidate, and worker nodes) to establish a [quorum](https://www.ibm.com/docs/en/spectrum-scale/5.1.5?topic=failure-quorum#nodequo) and maintain data consistency in the event of a node failure. Therefore, the minimum value of 1 may need to be larger if the value specified for management_node_count is less than 2. | No | 0 |
| `worker_node_instance_type` | Specify the virtual server instance profile type name to be used to create the worker nodes for the Spectrum LSF cluster. The worker nodes are the ones where the workload execution takes place and the choice should be made according to the characteristic of workloads. For choices on profile types, see [Instance Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles&interface=ui). Note: If dedicated_host_enabled == true, the available instance prefix (for example, bx2 and cx2) can be limited depending on your target region. Check `ibmcloud target -r {region_name}; ibmcloud is dedicated-host-profiles. | No | "bx2-4x16" |
| `worker_node_max_count` | The maximum number of worker nodes that can be deployed in the Spectrum LSF cluster. In order to use the [Resource Connector](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=lsf-resource-connnector) feature to dynamically create and delete worker nodes based on workload demand, the value that is selected for this parameter must be larger than worker_node_min_count. If you plan to deploy only static worker nodes in the LSF cluster, for example, when using Spectrum Scale storage, the value for this parameter should be equal to worker_node_min_count. Enter a value in the range 1 - 500. | No | 10 |
| `zones` | The IBM Cloud zone name within the selected region where the IBM Cloud HPC cluster should be deployed and requires a single zone input value. Supported zones are: eu-de-2 and eu-de-3 for eu-de, us-east-1 and us-east-3 for us-east, and us-south-1 for us-south. The management nodes file storage shares, and compute nodes are deployed in the same zone.[Learn more](https://cloud.ibm.com/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region#get-zones-using-the-cli). | No | ["us-east-1"] |







































































































{: caption="Deployment values" caption-side="top"} 
