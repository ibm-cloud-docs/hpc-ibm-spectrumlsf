---

copyright:
  years: 2025
lastupdated: "2025-01-06"

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

# Enable dedicated hosts
{: #enable-dedicated-hosts}

{{site.data.keyword.cloud}} dedicated hosts are physical servers that are committed to a group of users. Dedicated hosts offer virtual server provisioning capacity and maximum placement control.

Foloowing are the key values for enabling dedicated host:

* Dedicated host ensures no co-tenancy with other customers offering enhanced privacy and security. It provides isolation at the hardware level, which is critical for sensitive workloads or regulated environments.

* Dedicated hosts are ideal for workloads with consistent and high-performance requirements for long-running workloads.

* It allows you to manage where your VSIs are placed within a host, offering better control over workload distribution.

* High-security environments requiring physical isolation.

* Compliance-driven industries with strict data isolation and residency requirements.

By setting the `enable_dedicated_host` value to true, a dedicated host will be deployed and ensures that all the static worker nodes will be created on the dedicated host.

## Configuring dedicated host deployment values
{: #config-dedicated-hosts-deploy-values}

To enable dedicated host on a LSF cluster, the following variable needs to be defined:

| Dedicated Host variable | Description | Example value |
| ----- | ----------- | --------------- | ------------ |
| `enable_dedicatedhost` | Set this option to true to enable dedicated hosts for the VSI created for workload servers, with the default value set to false. | true |

To learn more about dedicated host, click [here](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=ui).
