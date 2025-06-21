---

copyright:
  years: 2025
lastupdated: "2025-06-21"

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
{: #fixpack}

A Fix Pack is a cumulative update package that includes repository files, security enhancements, vulnerability patches, updated resource connectors, and other improvements. These updates are designed to ensure the stability, security, and performance of IBM® Spectrum LSF deployments.

As of May 09, 2025, IBM officially released a new version of Fix Pack 15, which includes the latest critical fixes, enhancements, and compatibility updates tailored for evolving workload demands. IBM Spectrum LSF currently supports both Fix Pack 14 (FP14) and Fix Pack 15 (FP15).

By default, the IBM Spectrum LSF solution now ships with Fix Pack 15 (FP15) to provide users with the most up-to-date features and support. However, in recognition of existing deployments and customer needs, the solution also continues to fully support Fix Pack 14 (FP14). This backward compatibility ensures customers can maintain stable cluster environments while planning or performing upgrades at their convenience.

To maintain operational efficiency and security, IBM recommends keeping your cluster environment up-to-date with the latest supported Fix Pack unless specific version dependencies are in place for your workloads.

The following table shows the different images used for FP14 and FP15:

| LSF version | Deployer node | Management node | Login node | Compute node |
| ----- | ----------- | --------------- | ------------ | ------------ |
| Fix Pack 14 | "hpc-lsf-fp14-deployer-rhel810-v1" | "hpc-lsf-fp14-rhel810-v1" | hpc-lsf-fp14-compute-rhel810-v1 | hpc-lsf-fp14-compute-rhel810-v1 |
| Fix Pack 15 | "hpc-lsf-fp15-deployer-rhel810-v1" | "hpc-lsf-fp15-rhel810-v1" | hpc-lsf-fp15-compute-rhel810-v1 | hpc-lsf-fp15-compute-rhel810-v1 |
{: caption="Fix Pack images" caption-side="bottom"}

The same image is now used across login nodes, compute nodes, and dynamic worker nodes in the IBM Spectrum LSF solution.
{: note}

## Scenarios
{: #fixpack-scenario}

If you set the `lsf_version` to FP14, corresponding FP14 images must be used across all the LSF cluster nodes (Deployer/Login/Management/Compute). The same applies for FP15. Using mismatched images for different LSF versions, result in deployment failure, as required packages may be missing.

**For example:** When the `lsf_version` is set as fixpack_15 and the image under `login_instance` is set as "hpc-lsf-fp14-compute-rhel810-v1" then our automation ensures to validate and throws an error message. So always ensure that the images are set based upon the required fixpack version.

**Error message:**

```pre
Error: Invalid value for variable
│
│   on terraform.tfvars line 69:
│   69: login_instance = [{
│   70:    image   = "hpc-lsf-fp14-compute-rhel810-v1"
│   71:    profile = "bx2-2x8"
│   72: }]
│     ├────────────────
│     │ var.login_instance is list of object with 1 element
│     │ var.lsf_version is "fixpack_15"
│
│ Mismatch between login_instance image and lsf_version. Use an image with 'fp14' only when lsf_version is fixpack_14, and 'fp15' only with fixpack_15.
│
│ This was checked by the validation rule at variables.tf:249,3-13.
╵
```

## Post deployment validations
{: #fixpack-validations}

* If the deployment is done using the FP14 images, then you can see the lsid output as:

```pre
[lsfadmin@tue-test14-and-mgmt-1-3271-001 ~]$ lsid
IBM Spectrum LSF 10.1.0.14, Jan 14 2025
Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.14
Copyright International Business Machines Corp. 1992, 2016.
US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.

My cluster name is tue-test14-and
My master name is tue-test14-and-mgmt-1-3271-001.comp.com
```

* If the deployment is done using the FP15 images, then you can see the lsid output as:

```pre
[root@test-fi-mgmt-1-c613-001 ~]# lsid
IBM Spectrum LSF 10.1.0.15, Apr 14 2025
Suite Edition: IBM Spectrum LSF Suite for Enterprise 10.2.0.15
Copyright International Business Machines Corp. 1992, 2016.
US Government Users Restricted Rights - Use, duplication or disclosure restricted by GSA ADP Schedule Contract with IBM Corp.

My cluster name is test-fi
My master name is test-fi-mgmt-1-c613-001.lsf.com
```

## Conclusion
{: #fixpack-conclusion}

While both Fix Pack 14 (FP14) and Fix Pack 15 (FP15) are supported in the IBM Spectrum LSF solution, there are differences in the default feature enablement.

For both FP14 and FP15, Application Center and Process Manager are enabled by default to support job submission, workflow management, and monitoring.

However, with FP15, an additional enhancement is included — Web Services are also enabled by default. This provides out-of-the-box support for RESTful APIs, enabling seamless integration with external tools and automation frameworks. This makes FP15 a more complete and integration-ready package for modern workload environments.
