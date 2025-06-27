---

copyright:
  years: 2025
lastupdated: "2025-06-27"

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

# Enabling a VPN
{: #enable-vpn}

A VPN helps connect your on-premises network to the {{site.data.keyword.vpc_short}} network, which enables secure communication between the two environments.

The custom image automation supports creating a VPN gateway if you set the `vpn_enabled` value as true (by default, it is set to false). After the automation creates the VPN gateway, you need to create VPN connections, and any other security group rule changes as necessary, under that VPN gateway. See the {{site.data.keyword.vpc_short}} documentation for details on [adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections&interface=ui).

If you are enabling a VPN, you do not require creating floating IP addresses. For example, if you enabled a VPN, to connect to VSI-1 without a floating IP address, run:
```text
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null root@${packer_vsi_1_private_ip}
```
{: codeblock}
