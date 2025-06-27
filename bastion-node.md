---

copyright:
  years: 2025
lastupdated: "2025-06-27"

keywords:
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

# Bastion node
{: #bastion-node-overview}

A bastion node also known as a jump server is a secure entry point that is designed to provide controlled access to private network resources. It acts as an agent between the public internet and internal systems, allowing users to securely access internal servers without exposing sensitive resources directly to the public web.

The {{site.data.keyword.spectrum_full_notm}} solution includes support for bastion nodes as part of its architectural design. By default, a bastion node is created to facilitate secure access to the cluster nodes. Users can use this bastion node to connect to login, management, and worker nodes within the cluster. For enhanced security, SSH connections are only permitted through the bastion node, which then provides access to other cluster nodes.

The solution uses an Ubuntu-based operating system for the bastion node. To maintain security compliance and ensure up-to-date features, the automation code is regularly updated to deploy the latest Ubuntu 22.04 version.

It is not required to create a bastion node for every new deployment. If an existing bastion node is available, it can be reused to access cluster nodes.
{: note}

## Bastion node usage
{: #bastion-node-usage}

1. Enable bastion node through Architectural Design - The solution provisions a bastion node by default to secure access to cluster nodes.
2. Support for existing bastion setup - Users can leverage an existing bastion node instead of creating a new one, providing flexibility and continuity in secure cluster access.

## Default bastion node support
{: #bastion-node-support}

The LSF solution supports the creation of a bastion node by default for each individual deployment. When a new cluster is deployed, a bastion node is provisioned alongside the worker nodes based on the architecture setup. Users can securely access these cluster nodes through the bastion node.

As part of the VPC design, a dedicated subnet is created specifically for the bastion node. Also, a dedicated security group is assigned to the bastion node, which is configured to allow SSH access for secure connectivity to the cluster nodes. Internal automation ensures seamless communication between the bastion node (jump host) and the cluster nodes.

To trigger the creation of a bastion node for a fresh deployment, set the variable `bastion_instance_name` to null. When this value is detected, automation processes are initiated to provision the bastion node, enabling access to other cluster nodes.

Newly created bastion nodes are not automatically registered in the DNS domain for name resolution. As a result, access to these nodes can only be performed by using their IP addresses.
{: note}

## Support for existing bastion node
{: #bastion-node-existing-support}

The solution is designed to accommodate scenarios where an existing bastion node is already configured. This flexibility is useful when:

* A bastion node that has already been set up as part of a previous deployment.

* The environment uses LDAP-based authentication.

For instance, if a Spectrum Scale Storage cluster is already deployed and configured. The solution is associated with a bastion node already, users can repurpose this bastion node to manage and access the LSF cluster nodes. This provides a single, secure access point for both solutions.

To configure and use an existing bastion node, users must provide the following details:

* `existing_bastion_instance_name`: The hostname of the pre-configured bastion node.

* `existing_bastion_instance_public_ip`: The public IP address of the bastion node.

* `existing_bastion_security_group_id`: The security group associated with the bastion node. This ensures that the security group for the LSF cluster nodes allows traffic from the bastion node.

Failing to provide a correct security group ID or leaving the value as empty, the deployments will fail.
{: note}

* `existing_bastion_ssh_private_key`: The private SSH key (for example, id_rsa) used during the initial creation of the bastion node. This key is required for validation and for running remote operations during the cluster setup process. For more information, see [Getting started with SSH keys](https://cloud.ibm.com/docs/vpc?topic=vpc-ssh-keys&interface=ui).

By providing these details, the LSF cluster can be configured to use the existing bastion node, enabling secure access and efficient management.

This approach ensures a seamless and secure solution either by using a newly created or existing bastion node.
