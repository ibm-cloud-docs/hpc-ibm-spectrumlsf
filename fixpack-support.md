---

copyright:
  years: 2026
lastupdated: "2026-03-20"

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

# Fix Pack 15
{: #fixpack-overview}

A Fix Pack is a cumulative update package that includes repository files, security enhancements, vulnerability patches, updated resource connectors, and other improvements. These updates are designed to ensure the stability, security, and performance of {{site.data.keyword.spectrum_full}} deployments.

To maintain operational efficiency and security, IBM recommends keeping your cluster environment up-to-date with the latest supported Fix Pack unless specific version dependencies are in place for your workloads.

The following table shows the different images used for FP15:

| LSF version | Deployer node | Management node | Login node | Compute node |
| ----- | ----------- | --------------- | ------------ | ------------ |
| Fix Pack 15 | hpc-lsf-fp15-deployer-rhel810-v3 | hpc-lsf-fp15-rhel810-v3 | hpc-lsf-fp15-compute-rhel810-v3 | hpc-lsf-fp15-compute-rhel810-v3 |
{: caption="Fix Pack images" caption-side="bottom"}

The same compute images are used across login nodes, static compute nodes, and dynamic compute nodes in the {{site.data.keyword.spectrum_full}} solution.
{: note}

## Post deployment validations
{: #fixpack-validation}

If the deployment is done using the FP15 images, then you can see the lsid output as:

```pre
[root@test-fi-mgmt-1-c613-001 ~]# lsid
IBM Spectrum LSF 10.1.0.15, Apr 14 2025
Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
Copyright International Business Machines Corp. 1992, 2016.
US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.

My cluster name is test-fi
My master name is test-fi-mgmt-1-c613-001.hpc.local
```

## Conclusion
{: #fixpack-conclusion}

However, with FP15, an additional enhancement is included — Web Services and License Scheduler are also enabled by default. This makes FP15 a more complete and integration-ready package for modern workload environments.
