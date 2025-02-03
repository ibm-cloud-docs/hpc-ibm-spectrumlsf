---

copyright:
  years: 2025
lastupdated: "2025-02-03"

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

{{site.data.keyword.monitoringlong_notm}} is a cloud-native and container-intelligence management system that is include as part of {{site.data.keyword.cloud_notm}} architecture. The Cloud Monitoring can be used to gain operational visibility into the performance and health of your applications, services, and platforms. It offers administrators, DevOps, and developers a full-stack telemetry with advanced features to monitor, troubleshoot, define alerts, and design custom dashboards.

To add the monitoring features with Cloud Monitoring in the {{site.data.keyword.cloud_notm}}, you must provision an instance of this service.

| Value | Description | Type | Default value | Validation |
| ----- | ----------- | --------------- | ------------ | ------------ |
| `observability_monitoring_enable` | Set this value as "false" to disable the {{site.data.keyword.monitoringlong_notm}} integration. If enabled, infrastructure and LSF application metrics from management nodes will be ingested. | bool | true |
| `observability_monitoring_on_compute_nodes_enable` | Set this value as "false" to disable {{site.data.keyword.monitoringlong_notm}} integration. If enabled, infrastructure metrics from compute nodes will be ingested. | bool | false |
| `observability_monitoring_plan` | Type of service plan for {{site.data.keyword.monitoringlong_notm}} instance. You can choose one of the following: lite or graduated-tier. For more information, refer the [IBM Cloud Monitoring Service Plans](/docs/monitoring?topic=monitoring-service_plans). | string | "graduated-tier" | * Condition: Validates if the value matches lite or graduated-tier.  \n * Error Message: "Please enter a valid plan for {{site.data.keyword.monitoringlong_notm}}, for all details visit https://cloud.ibm.com/docs/monitoring?topic=monitoring-service_plans." |
| `observability_enable_metrics_routing` | Enable the metrics routing to manage metrics at the account level by configuring targets and routes that define where data points are routed. | bool | false |
{: caption="{{site.data.keyword.monitoringlong_notm}} variables" caption-side="bottom"}

You can use {{site.data.keyword.metrics_router_full_notm}}, a platform service to manage metrics at the account-level by configuring targets and routes that define where data points are routed.

To check if Cloud Monitoring is configured correctly on your VSI, SSH into the instance and run the following commands:

```
systemctl status prometheus
systemctl status dragent
```
{: pre}

## Key features
{: #key-features}

Following are the key features of {{site.data.keyword.monitoringlong_notm}}:

* Consolidate time series data to the region of your primary operations.
* Route time series data to one or multiple locations.
* Improve your data residency compliance stature, keeping data at-rest within certain regions.

For more information on {{site.data.keyword.monitoringlong_notm}}, refer the following documentation links:
* [About IBM Cloud Metrics Routing in IBM Cloud](/docs/metrics-router?topic=metrics-router-about&interface=ui)
* [Getting started with IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-getting-started)
