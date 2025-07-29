---

copyright:
  years: 2025
lastupdated: "2025-07-29"

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

# Enabling a Virtual Private Network (VPN)
{: #enable-vpn}

A VPN provides a secure and encrypted connection between your on-premises network and the IBM Cloud Virtual Private Cloud (VPC) environment. This allows seamless communication across both environments as if they are part of the same private network.
Once the VPN gateway is provisioned through automation, you must configure the necessary VPN connections to establish the actual data path between the networks. In addition, review and update any required security group rules or routing settings to ensure proper traffic flow and access control. For more information, see the {{site.data.keyword.vpc_short}} documentation on [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections&interface=ui).

## Creating VPN gateway (Automation)
{: #vpn-automation}

During the automation process, a VPN gateway is created in your IBM Cloud VPC. This gateway serves as the termination point for VPN tunnels coming from your on-prem network. For more information, see [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway&interface=ui).

## Steps (After creating VPN gateway)
{: #after-gateway-creation}

Below are the steps after creating the VPN gateway:

1. **Create VPN Connections -** Define the actual VPN tunnels by creating connections under the VPN gateway. This includes specifying peer IP addresses, pre-shared keys, and IKE/IPSec policies.

2. **Configure Routing -** Ensure that appropriate routes are added to route tables in the VPC subnet(s) to forward traffic to the VPN gateway.

3. **Update Security Group Rules -** Modify or add security group rules to allow traffic from your on-premises IP ranges to the required VPC resources (for example, port 22 for SSH, port 443 for HTTPS).

4. **Network ACLs and Firewall Rules -** If Network ACLs or additional firewalls (on-prem or in-cloud) are in use, ensure that they are configured to permit VPN traffic.
