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

# IBM Cloud Monitoring
{: #cloud-monitoring-overview}

{{site.data.keyword.monitoringlong_notm}} is a cloud-native, and container-intelligence management system that you can include as part of your {{site.data.keyword.cloud_notm}} architecture. Use it to gain operational visibility into the performance and health of your applications, services, and platforms. It offers administrators, DevOps teams and developers full-stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.

To add monitoring features with {{site.data.keyword.monitoringlong_notm}} in the {{site.data.keyword.cloud_notm}}, you must provision an instance of the {{site.data.keyword.monitoringlong_notm}} service.

| Value | Description | Type | Default value | Validation |
| ----- | ----------- | --------------- | ------------ | ------------ |
| `observability_monitoring_enable` | Set false to disable {{site.data.keyword.monitoringlong_notm}} integration. If enabled, infrastructure and LSF application metrics from Management Nodes will be ingested. | bool | true |
| `observability_monitoring_on_compute_nodes_enable` | Set false to disable {{site.data.keyword.monitoringlong_notm}} integration. If enabled, infrastructure metrics from Compute Nodes will be ingested. | bool | false |
| `observability_monitoring_plan` | Type of service plan for {{site.data.keyword.monitoringlong_notm}} instance. You can choose one of the following: lite, graduated-tier. For all details visit [IBM Cloud Monitoring Service Plans](/docs/monitoring?topic=monitoring-service_plans). | string | "graduated-tier" | * Condition: Validates if the value matches lite or graduated-tier.  \n * Error Message: "Please enter a valid plan for {{site.data.keyword.monitoringlong_notm}}, for all details visit https://cloud.ibm.com/docs/monitoring?topic=monitoring-service_plans." |
| `observability_enable_metrics_routing` | Enable metrics routing to manage metrics at the account level by configuring targets and routes that define where data points are routed. | bool | false |
{: caption="{{site.data.keyword.monitoringlong_notm}} vairables" caption-side="bottom"}

You can use {{site.data.keyword.metrics_router_full_notm}}, a platform service, to manage metrics at the account-level by configuring targets and routes that define where data points are routed.

## Key features IBM Cloud Monitoring
{: #key-features}

Following are the key features of {{site.data.keyword.monitoringlong_notm}}:

* Consolidate time series data to the region of your primary operations
* Route time series data to one or multiple locations
* Improve your data residency compliance stature, keeping data at-rest within certain regions

For more information on {{site.data.keyword.monitoringlong_notm}}, refer the following documentation links:
* [About IBM Cloud Metrics Routing in IBM Cloud](/docs/metrics-router?topic=metrics-router-about&interface=ui)
* [Getting started with IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-getting-started)
