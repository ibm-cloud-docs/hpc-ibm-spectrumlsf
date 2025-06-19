---

copyright:
  years: 2025
lastupdated: "2025-06-19"

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

# Fix Pack support
{: #fixpack-overview}

Fix Pack is the cumulative package that contains the repository files, security fixes, vulnerabilities fixes, resource connectors, and so on. {{site.data.keyword.spectrum_full}} supports both Fix Pack 14 (FP14) and Fix Pack 15 (FP15) packages.

In this release, the LSF cluster deployments are done using Fix Pack 14 and Fix Pack 15.

The following table shows the different images used for FP14 and FP15:

| LSF version | Deployer node | Management node | Login node | Compute node |
| ----- | ----------- | --------------- | ------------ | ------------ |
| Fix Pack 14 | "hpc-lsf-fp14-deployer-rhel810-v1" | "hpc-lsf-fp14-rhel810-v1" | hpc-lsf-fp14-compute-rhel810-v1 | hpc-lsf-fp14-compute-rhel810-v1 |
| Fix Pack 15 | "hpc-lsf-fp15-deployer-rhel810-v1" | "hpc-lsf-fp15-rhel810-v1" | hpc-lsf-fp15-compute-rhel810-v1 | hpc-lsf-fp15-compute-rhel810-v1 |
{: caption="Fix Pack images" caption-side="bottom"}

The images used for login node, compute node, and dynamic nodes are the same.
{: note}

If you are using the `lsf_version` as FP14, then by default FP14 images should be used. This implies same for FP15 also. If you are using different images for different LSF versions, then the deployment fails, stating that some of the packages are not available.

**Example:**
![Images and LSF versions example](images/example_LSF_versions.png "Images and LSF versions example"){: caption="Images and LSF versions example" caption-side="bottom"}

**Post deployment validations:**

* If the deployment is done using the FP14 images, then you can see the LSID output as:

```pre
[lsfadmin@tue-test14-and-mgmt-1-3271-001 ~]$ lsid
IBM Spectrum LSF 10.1.0.14, Jan 14 2025
Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.14
Copyright International Business Machines Corp. 1992, 2016.
US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.

My cluster name is tue-test14-and
My master name is tue-test14-and-mgmt-1-3271-001.comp.com
```

* If the deployment is done using the FP15 images, then you can see the LSID output as:

```pre
[root@test-fi-mgmt-1-c613-001 ~]# lsid
IBM Spectrum LSF 10.1.0.15, Apr 14 2025
Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
Copyright International Business Machines Corp. 1992, 2016.
US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.

My cluster name is test-fi
My master name is test-fi-mgmt-1-c613-001.lsf.com
```
