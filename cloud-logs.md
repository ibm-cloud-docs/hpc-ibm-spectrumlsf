---

copyright:
  years: 2025
lastupdated: "2025-01-17"

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

# IBM Cloud Logs
{: #cloud-logs-overview}

{{site.data.keyword.logs_full_notm}} is a scalable logging service which is designed to persist logs while providing users with robust capabilities for querying, tailing, and visualising their logs efficiently. This service supports enhanced observability for both management and compute resources in the {{site.data.keyword.cloud_notm}} environment.

## Key features
{: #key-features}

Following are the key features of {{site.data.keyword.logs_full_notm}}:

* Scalable log storage and persistence.

* Advanced querying, tailing, and visualisation capabilities.

* Integration options for management and compute Virtual Server Instances (VSIs).

## Functionality
{: #cloud-log-functionality}

Users can enable cloud logs to capture infrastructure and application logs from both management and compute nodes. Following are the four new variables introduced:

* `observability_logs_enable_for_management`: Set this value as "false" to disable the {{site.data.keyword.logs_full_notm}} integration. If enabled, infrastructure and LSF application logs from management nodes will be ingested.

* `observability_logs_enable_for_compute`: Set this value as "false" to disables the {{site.data.keyword.logs_full_notm}} integration. If enabled, infrastructure and LSF application logs from compute nodes will be ingested.

* `observability_enable_platform_logs`: Setting this value as "true" creates a tenant in the same region in which the {{site.data.keyword.logs_full}} instance is provisioned to enable platform logs for that region. 

    You can have only one tenant per region in an account.
    {: note}

* `observability_logs_retention_period`: The number of days {{site.data.keyword.logs_full_notm}} retains the logs data in priority insights. The allowed values are 7, 14, 30, 60, and 90.

## Verifying Log Flow
{: #verify-log-flow}

To ensure that the logs are successfully flowing to the {{site.data.keyword.logs_full_notm}} instance, test messages are sent through userdata.

The dashboard results in a visual confirmation of logs ingestion and flow.

![Architecture diagram.](images/verifying_log_flow.png "Verifying the log flow"){: caption="Verifying the log flow" caption-side="bottom"}

## Using Filters
{: #using-filters}

Users can apply filters based on the subsystem and application to refine the logs:

* To view the logs from management nodes only, select the **management** subsystem.

* To view the logs from compute nodes only, select the **compute** subsystem.

If your log instance is also configured as a target for {{site.data.keyword.atracker_short}}, additional application names may appear besides "LSF". To exclude audit events, filter by the LSF application names specifically.

## Supported Operating Systems
{: #supported-operating-sys}

The supported operating systems for {{site.data.keyword.logs_full_notm}} are RHEL and Ubuntu.

For more information on {{site.data.keyword.logs_full_notm}}, go to the documentation [here](/docs/cloud-logs?topic=cloud-logs-getting-started).
