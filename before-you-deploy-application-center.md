---

copyright:
  years: 2025
lastupdated: "2025-01-21"

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

# Using {{site.data.keyword.cloud_notm}} HPC with LSF Application Center and high availability
{: #before-deploy-application-center}

Enable LSF Application Center with your {{site.data.keyword.cloud}} HPC cluster [during deployment](/docs-draft/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-deploy-architecture) by setting `enable_app_center` to **true**, and `app_center_gui_pwd` to match your LSF Application Center password. High availability for LSF Application Center is enabled by default and managed with the `app_center_high_availability` deployment input value (that is, it is set to **true** by default). Leaving this high availability input value enabled allows LSF Application Center to:
* run on all deployed management nodes.
* use a cross availability zone instance of the [{{site.data.keyword.cloud}} Database for MySQL](/docs/databases-for-mysql?topic=databases-for-mysql-getting-started) as the backend database.
* use an [{{site.data.keyword.cloud}} Application Load Balancer for VPC (ALB)](/docs/vpc?topic=vpc-load-balancers-about) as the VPC load balancer to dispatch requests to LSF Application Center nodes.

Before you can deploy your {{site.data.keyword.cloud_notm}} HPC cluster with LSF Application Center in high availability mode, import a TLS termination certificate and authorize it.

## Importing a TLS termination certificate in Secrets Manager and configuring authorization
{: #importing-certificate}

For LSF Application Center high availability, you require a certificate for TLS termination from the VPC load balancer with HTTPS connections. Even self-signed certificates are supported, but you need to accept them in a browser. Import this certificate to [{{site.data.keyword.cloud}} Secrets Manager](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui) and provide the appropriate authorization:

1. Obtain the certificate to be used for TLS termination of your LSF Application Center. The certificate domain must match the value that is specified for the `dns_domain_names` deployment input value. It can be either be a wildcard domain (such as ***.mydomainname**) or can contain a DNS alias (such as **pac.mydomainname**).

2. Create a secret in [{{site.data.keyword.cloud}} Secrets Manager](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui) for the certificate.

3. [Import the certificate](/docs/secrets-manager?topic=secrets-manager-certificates&interface=ui) to Secrets Manager.

4. Take note of the CRN (cloud resource name) value for the certificate, as you need to provide it during {{site.data.keyword.cloud}} HPC cluster deployment as the `certificate_instance` deployment input value. The VPC load balancer front-end listeners use the CRN to load the certificate.

5. Provide IAM service to service authorization between the VPC load balancer resource type and the Secrets Manager user instance. This way, the VPC load balancer for your {{site.data.keyword.cloud_notm}} HPC cluster instance can retrieve the TLS certificate and related private key in a secure and auditable way. For example:
    1. In the {{site.data.keyword.cloud_notm}} console, select **Manage > Access (IAM) > Authorizations** to display the **Grant a service authorization** page.
    2. In the **Source** section, select **This account** as the account for the service.
    3. Select **VPC Infrastructure Services** as the service.
    3. Select **Specific resources** as the scope.
    4. Select **Resource Type** in the **Add attributes** section, and select **Load Balancer for VPC**.
    5. In the **Target** section, select **Secrets Manager** as the source to which to give access.
    6. Select **Instance ID**, select the **string equals**, and select your Secrets Manager instance.
    7. In the **Roles** section, select **Writer** as the level of access to assign.

With these settings complete, you can [deploy your {{site.data.keyword.cloud_notm}} HPC cluster](/docs/allowlist/hpc-service?topic=hpc-service-deploy-architecture&interface=ui) with LSF Application Center high availability enabled. Once deployed, you can [access the LSF Application Center](/docs/allowlist/hpc-service?topic=hpc-service-accessing-lsf-gui).
