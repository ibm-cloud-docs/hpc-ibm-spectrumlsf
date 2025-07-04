---

copyright:
  years: 2025
lastupdated: "2025-06-27"

keywords: vpc, lsf

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

# Virtual Private Clouds (VPCs)
{: #vpc}

You can choose to deploy the Spectrum LSF solution by creating a new VPC or using an existing VPC and existing subnets.
{: shortdesc}

You can use {{site.data.keyword.vpc_full}} as your VPC. {{site.data.keyword.vpc_short}} supports creating your own space in {{site.data.keyword.cloud}} for a secure, isolated virtual network that combines the security of a private cloud with the availability and scalability of {{site.data.keyword.IBM_notm}}'s public cloud. {{site.data.keyword.vpc_short}} gives your applications logical isolation from other networks, and provides scalability and security. To make this logical isolation possible, the VPC is divided into subnets that use a range of private IP addresses. You can create subnets in suggested prefix ranges, or bring your own public IP address range (BYOIP) to your IBM Cloud account. By default, all resources within the same VPC can communicate with each other over the private network, regardless of their subnet.

For more details about {{site.data.keyword.vpc_short}}, see the [{{site.data.keyword.vpc_short}} documentation](/docs/vpc?topic=vpc-about-vpc).

## Using a new VPC for your {{site.data.keyword.spectrum_full_notm}} cluster
{: #vpc-new}

If you choose to create a new VPC, set the `vpc_name` input value as null during the Spectrum LSF cluster deployment. With this setting, the {{site.data.keyword.spectrum_full}} cluster deployment automatically creates a brand-new VPC by using the provided address prefix that you provide for the `vpc_cidr` input value. Make sure that you provide a valid address prefix for the `vpc_cidr` input value.

With a new VPC, the {{site.data.keyword.spectrum_full}} cluster deployment automatically isolates the network, and creates two different subnets under the new VPC by using the `vpc_cidr` value:

* It splits the larger CIDR range from in `vpc_cidr`, into two different networks ranges based on number of IP addresses needed under that subnet.

* After the CIDR ranges are passed in the `vpc_cidr`, `vpc_cluster_private_subnets_cidr_blocks`, and `vpc_cluster_login_private_subnets_cidr_blocks` input values, the {{site.data.keyword.spectrum_full}} cluster deployment automatically creates the VPC and subnets. One subnet range with the same CIDR range is used only for the creation of bastion and login nodes. The other subnets are used to create management nodes or VPC file shares and compute nodes.

   Since on the `vpc_cluster_login_private_subnets_cidr_blocks` value is used to create only the bastion and login nodes, use a smaller CIDR. If a larger range is specified, they go unused.
   {: tip}

## Using an existing VPC for your {{site.data.keyword.spectrum_full_notm}} cluster
{: #vpc-existing}

If you have existing VPC infrastructure, you can use that VPC for your {{site.data.keyword.spectrum_full_notm}} cluster. There are two possible approaches to using an existing VPC:

### An existing VPC with existing subnets available to use
{: #existing-vpc}

If you use your existing VPC for your {{site.data.keyword.spectrum_full}} cluster, set the `vpc_name` input value during {{site.data.keyword.spectrum_full}} cluster deployment with the name of your existing VPC. With this setting, the {{site.data.keyword.spectrum_full}} cluster deployment automatically skips creating a new VPC and uses the one you specify and its existing VPC details for all networking.

With an existing VPC, you can also choose to use existing subnets to create {{site.data.keyword.spectrum_full_notm}} cluster nodes. Cluster deployment needs two subnets:

* Provide a larger subnet ID for the `cluster_subnet_id` deployment input value, as it is used to create all management nodes or VPC file shares, and the compute nodes.
* Provide another subnet ID for the `login_subnet_id` to create the bastion and login nodes.

### An existing VPC and automatically creating two new subnets from the {{site.data.keyword.spectrum_full}} cluster deployment
{: #existing-two-subnets}

If you have an existing VPC but there are no existing subnets to use, then provide the available valid CIDR range for the `vpc_cluster_private_subnets_cidr_blocks` and `vpc_cluster_login_private_subnets_cidr_blocks` {{site.data.keyword.spectrum_full}} cluster deployment input values. The deployment creates two new subnets under your provided existing VPC. When a new VPC is created, subsequent VPC IDs are attached as an allowed network under the DNS zones. Custom resolvers can also resolve all the DNS entries for the traffic that originates from VPC or subnets.

* Provide a smaller CIDR range for `vpc_cluster_login_private_subnets_cidr_blocks` for the creation of bastion and login nodes. Provide a bigger range of CIDR under `vpc_cluster_private_subnets_cidr_blocks` for the creation of management nodes/VPC file shares/compute nodes.

When you provide existing VPC detail, subsequent VPC IDs are attached as an allowed network under the DNS zones. Custom resolvers can also resolve all the DNS entries for the traffic that originates from VPC or subnets.

Always the name of the VPC variable is a name and the ID is the ID and not the CRN.
{: note}
