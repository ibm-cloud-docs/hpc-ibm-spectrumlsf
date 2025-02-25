---

copyright:
  years: 2024
lastupdated: "2025-02-25"

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

With user-managed encryption, you can bring your own custom root key (CRK) to the cloud or have a key management service (KMS) generate a key for you. You use root keys to encrypt resources across regions. You can encrypt resources with a key that is stored in your regional KMS instance, and you can use root keys from another region.
{: shortdesc}

## {{site.data.keyword.keymanagementservicelong_notm}} instances for your {{site.data.keyword.spectrum_full_notm}} cluster
{: #keys}

Use an {{site.data.keyword.keymanagementservicelong_notm}} instance regardless of whether you have the {{site.data.keyword.spectrum_full_notm}} deployment process create one for you or integrate an existing one.

### Creating an {{site.data.keyword.keymanagementservicelong_notm}} instance and key
{: #new-key}

Automatically encrypt infrastructure resources through {{site.data.keyword.keymanagementservicelong_notm}} for your {{site.data.keyword.spectrum_full_notm}}. To enable this feature for your cluster, always keep the `key_management` deployment input value as **key_protect** (which is the default). The deployment process creates an {{site.data.keyword.keymanagementservicelong_notm}} instance and a specific key to encrypt these resources:

* {{site.data.keyword.block_storage_is_full}} (Cloud Block Storage)
* {{site.data.keyword.filestorage_vpc_full}}
* {{site.data.keyword.cos_full}}

If the value for `key_management` is set as null, then the deployment process does not automatically create {{site.data.keyword.keymanagementservicelong_notm}} instances or keys and all infrastructure resources are encrypted through provider-managed encryption.

### Integrating an existing {{site.data.keyword.keymanagementservicelong_notm}} instance and key
{: #existing-key}

If you have an existing {{site.data.keyword.keymanagementservicelong_notm}} instance and an encryption key, set the `key_management` deployment input value as **key_protect**. Provide the instance name for the `kms_instance_name` and the encryption name for the `kms_key_name` deployment input variables. This way, the deployment process uses these values to encrypt all infrastructure resources for your {{site.data.keyword.spectrum_full_notm}} cluster.

## IAM service-to-service authorization for your {{site.data.keyword.spectrum_full_notm}} cluster
{: #iam}

Use {{site.data.keyword.iamlong}} (IAM) to create or remove authorization that grants one service access to another service for your {{site.data.keyword.spectrum_full_notm}} cluster. This service authorization grants a source service or group of services in any account access to a target service or group of services in an account. You can also use authorization delegation to automatically create access policies that grant access to dependent services.

1. Enabling service-to-service authorization between the KMS and Cloud Block Storage

You can set the {{site.data.keyword.spectrum_full_notm}} cluster deployment process to automatically enable service-to-service authorization between your KMS and Cloud Block Storage service by setting the `skip_iam_block_storage_authorization_policy` deployment input value as **false**. This way, the process automatically creates service authorization between Cloud Block Storage and the IBM Key Protect instance ID. This happens when the KMS instance is created through automation.

When you use an existing kms instance, which already has an autorisation that is enabled between Cloud Block Storage and the KMS instance ID, then set the `skip_iam_block_storage_authorization_policy` deployment input value as **true**. In this case, the process skips creating a new service authorization. Also, when you use an existing KMS instance ID and if the authorization is not enabled, then keep the value as **false** (by default) so the solution establishes the authorization.

2. Enabling service-to-service authorization between the KMS and VPC File Storage service

You can set the {{site.data.keyword.spectrum_full_notm}} cluster deployment process to automatically enable service-to-service authorization between your KMS and VPC file storage service by setting the `skip_iam_share_authorization_policy` deployment input value as **false**. This way, the process automatically creates service authorization between Cloud Block Storage and the VPC file storage service. This happens when the KMS instance is created through automation.

When you use an existing KMS instance, which already has an autorisation that is enabled between VPC file storage and the KMS instance ID, then set the `skip_iam_share_authorization_policy` deployment input value as **true**. In this case, the process skips creating a new service authorization. Also, when you use an existing KMS instance ID and if the authorization is not enabled, then keep the value as **false** (by default) so the solution establishes the authorization.

If the service authorization exists and you create a new one, you can encounter a message similar to: `Error creating authorization policy: The policy wasn't created because an access policy with identical attributes and roles already exists.`
{: note}
