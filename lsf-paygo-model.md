---

copyright:
  years: 2025
lastupdated: "2025-11-04"

keywords: lsf, pay-as-you-go
subcollection: hpc-ibm-spectrumlsf

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}

# LSF Pay-As-You-Go (PAYGo) model
{: #payg-model-intro}

The LSF Pay-As-You-Go images are prebuilt virtual machine images available through the IBM Cloud Catalog. These images allow users to deploy LSF clusters without needing to provide their own licenses, eliminating the need for the Bring-Your-Own-License (BYOL) model.

## Key features
{: #key-aspects}

Following are the key aspects of LSF PayGo model:

1. Prelicensed and Metered

    * The licensing is integrated into the PAYGo image.
    * Billing is automatically managed through IBM Cloud metering.
    * Charges are based on number of vCPU hours consumed.

2. Quick deployment

    * PAYGo images are available directly in IBM Cloud.
    * Users can deploy the LSF clusters within minutes.
    * PAYGo images are used for management and compute nodes.

3. Simplified maintenance

    * The images are maintained, patched, and updated by the IBM team.
    * Users benefit from secure, tested, and up-to-date LSF versions.

4. Flexible scaling

    Ideal for dynamic and burst workloads with fluctuating compute demands.

Bastion and deployer nodes are not supported by PAYGo.
{: note}

## LSF PAYGo features
{: #paygo-feature}

Following are the features of LSF PayGo images:

### **PAYGo Pricing Model**
{: #paygo-pricing}

* The pricing is based on hourly vCPU usage.
* Eliminates upfront licensing costs.
* Users are billed proportionally to the compute resources consumed.

### **PAYGo Image Design**
{: #paygo-image-design}

* This model includes all necessary LSF FP15 components, RPMs, and configurations. It supports:

    * Management nodes
    * Static and dynamic worker nodes

* IBM builds, validates, and manages the image for consistent deployment.

* A backend pricing mechanism calculates costs based on vCPU cores.

### **PAYGo configuration variable**
{: #paygo-config-variable}

A new configuration variable `lsf_pay_per_use` is introduced as part of this design.

| Variable | Value | Description |
| ----- | ----------- | --------------- |
| `lsf_pay_per_use` | true (default) | Enables PAYGo billing and uses prebuilt images. Bring Your Own Image (BYOI) is not supported. Only LSF FP15 images are compatible. |
| `lsf_pay_per_use` | false | Disables PAYGo billing and allows BYOI and custom images. No pay-per-use charges apply. |
{: caption="PAYGo configuration variable" caption-side="bottom"}

The costs is based on:
* Selected hardware profile
* PAYGo image pricing
* Volume storage (only if separately provisioned and not part of the profile)

## Use case 1: PAYGo enabled (lsf_pay_per_use = true)
{: #payg-usecase1}

When the PAYGo feature is enabled (default setting), then:
* Automation provisions all the nodes by using the PAYGo image.
* Dynamic node provisioning is integrated through the LSF Resource Connector.
* Only LSF FP15 images are supported; FP14 or earlier are not compatible.
* BYOI is not supported.

## Use case 2: PAYGo mode disabled (lsf_pay_per_use = false)
{: #payg-usecase2}

When the PAYGo feature is disabled, then:
* Automation provision clusters by using default or user-provided custom images.
* Users must provide valid software licenses.
* Offers full control over image customization.
* BYOI is supported.

## Cluster validation
{: #cluster-validation}

* **When PAYGo is enabled:**
    * Instances are tagged with PAYGo pricing plans.
    * Check the **Image Details** section in the UI for confirmation.

Look for tags indicating FP15 PAYGo image usage and billing metrics.

* **When PAYGo is disabled:**
    * Instances are tagged with custom image identifiers.
    * PAYGo pricing plan is not applied.

## Billing and Usage
{: #lsf-billing}

To view the billing details, do the following:

1. Navigate to **Billing and Usage**.
2. Click **Usage** on the left side.
3. Search for **LSF Pay per Use** under the **Usage** list.
4. Click **View Plans** and select **Spectrum LSF Pay per Use** and choose **View Details**.
5. Select the desired month to view usage and pricing.
