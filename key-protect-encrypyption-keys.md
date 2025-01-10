---

copyright:
  years: 2024
lastupdated: "2025-01-10"

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

# {{site.data.keyword.keymanagementservicelong}} and encryption keys
{: #key-protect}

The [{{site.data.keyword.keymanagementservicefull}} ({{site.data.keyword.keymanagementservicelong_notm}})](/docs/key-protect) service helps you provision and store encrypted keys for applications across {{site.data.keyword.cloud_notm}} services, so you can see and manage data encryption and the entire key lifecycle from one central location.

With customer-managed encryption, you can bring your own custom root key (CRK) to the cloud or have a key management service (KMS) generate a key for you. You use root keys to encrypt resources across regions. You can encrypt resources with a key that is stored in your regional KMS instance, and you can use root keys from another region.
{: shortdesc}

## {{site.data.keyword.keymanagementservicelong_notm}} instances for your IBM Spectrum LSF cluster
{: #keys}

Use an {{site.data.keyword.keymanagementservicelong_notm}} instance regardless of whether you have the IBM Spectrum LSF deployment process create one for you or integrate an existing one.

### Creating an {{site.data.keyword.keymanagementservicelong_notm}} instance and key
{: #new-key}

Automatically encrypt infrastructure resources through {{site.data.keyword.keymanagementservicelong_notm}} for your IBM Spectrum LSF. To enable this feature for your cluster, always keep the `key_management` deployment input value as **key_protect** (which is the default). The deployment process creates an {{site.data.keyword.keymanagementservicelong_notm}} instance and a specific key to encrypt these resources:

* {{site.data.keyword.block_storage_is_full}} (Cloud Block Storage)
* {{site.data.keyword.filestorage_vpc_full}}
* {{site.data.keyword.cos_full}}

If the value for `key_management` is set as null, then the deployment process does not automatically create {{site.data.keyword.keymanagementservicelong_notm}} instances or keys and all infrastructure resources are encrypted through provider-managed encryption.

### Integrating an existing {{site.data.keyword.keymanagementservicelong_notm}} instance and key
{: #existing-key}

If you have an existing {{site.data.keyword.keymanagementservicelong_notm}} instance and an encryption key, set the `key_management` deployment input value as **key_protect**. Provide the instance name for the `kms_instance_name` and the encryption name for the `kms_key_name` deployment input variables. This way, the deployment process uses these values to encrypt all infrastructure resources for your IBM Spectrum LSF cluster.

## IAM service-to-service authorization for your IBM Spectrum LSF cluster
{: #iam}

Use {{site.data.keyword.iamlong}} (IAM) to create or remove authorization that grants one service access to another service for your IBM Spectrum LSF cluster. This service authorization grants a source service or group of services in any account access to a target service or group of services in an account. You can also use authorization delegation to automatically create access policies that grant access to dependent services.

### Enabling service-to-service authorization between the KMS and Cloud Block Storage
{: #new-service_auth}

You can set the IBM Spectrum LSF cluster deployment process to automatically enable service-to-service authorization between your KMS and Cloud Block Storage service by setting the `skip_iam_authorization_policy` deployment input value as **false**. This way, the process automatically creates service authorization between Cloud Block Storage and the {{site.data.keyword.keymanagementservicelong_notm}} instance ID.

### Integrating an existing service authorization
{: #existing-service-auth}

If you have an existing service authorization that is enabled between Cloud Block Storage and the KMS instance ID, then set the `skip_iam_authorization_policy` deployment input value as **true**. In this case, the process skips creating a new service authorization.

If the service authorization exists and you create a new one, you can encounter a message similar to: `Error creating authorization policy: The policy wasn't created because an access policy with identical attributes and roles already exists.`
{: note}
