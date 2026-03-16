---

copyright:
  years: 2026
lastupdated: "2026-03-16"

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

# Architecture and Workflow
{: #lsf-sch-architecture}

The LSF License Scheduler manages license tokens rather than controlling licenses directly. The jobs receive a license token before starting the application from the scheduler. The number of tokens available from {{site.data.keyword.spectrum_full}} is equal to the number of licenses available from the license server. If no token is available, then job does not start. This ensures that the number of licenses requested by running jobs does not exceed the number of available licenses.
When a job starts, the application is unaware of the **License Scheduler** and performs license checkout from the license server as usual.

## Workflow
{: #lsf-sch-workflow}

The following outlines the workflow of LSF License Scheduler:

* LSF License Scheduler enables LSF to gather license requirements from pending jobs, ensuring more efficient allocation of available licenses.
* LSF License Scheduler policies function separately from all other LSF scheduling policies.
* When a job starts, LSF applies basic scheduling before any other scheduling steps.
* LSF License Scheduler does not impact the job scheduling priority. Jobs are dispatched based on the prioritization policies defined for each cluster.
* LSF uses CPU time, run time, and resource usage to determine its other fair‑share policies.
* When LSF fair‑share scheduling is enabled, LSF first determines which user or queue has the highest priority, and then evaluates other resource requirements. As a result, other LSF fair-share policies have priority over LSF License Scheduler.

## License Scheduler modes
{: #lsf-modes}

When using License Scheduler, you must select either project mode or cluster mode for each license, depending on your needs. Each license feature can operate in either cluster mode or project mode, but not both. All license features required by a job must use the same mode.

By default, the system is configured to use cluster mode. Users can modify the `sf.licensescheduler` file to switch to project mode.
{: note}
