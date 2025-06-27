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

# Managing Process Manager using CLI
{: #about-process-manager-cli}

Perform the following steps to create, submit, and verify the jobs through Process Manager using CLI:

1. Connect to the LSF management node through SSH. The details are available in the Schematics log output with the following `ssh_to_management_node` variable:

    ```pre
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -J ubuntu@162.133.142.116 lsfadmin@10.241.16.6
    ```

2. Run the following commands to view the existing flow definitions:

    ```pre
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jdefs -u all
    NAME           USER          STATUS         FLOW_IDS
    test571       lsfadmin       OnHold         1(Done)
    hpc123        lsfadmin       OnHold         2(Done)
                                                3(Done)
                                                4(Done)
                                                5(Done)
    pm_test_flow   lsfadmin       OnHold        6(Killed)
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$
    ```

2. Create a flow definition file based on your requirements.

    The following example defines the flow with two jobs, where the second job starts only after the first job completes successfully. Save the content of the file with a **.xml** extension, For example: **pm_test_flow.xml**

    ```pre
    <?xml version="1.0" encoding="UTF-8"?>
    
    
    <!DOCTYPE JobFlowReq SYSTEM "JobFlowDef30.dtd"><JobFlowReq  DTDVersion="3.0"  MainEntry="pm_test_flow"  Description=""  ClientVersion="10.201"  ExitCodeRule="FromSumOfItems"  Exclusive="No"  FlowStateMailWhen="Exit"  FlowEnvVars=""  MailDestination="lsfadmin"  EndBehaviour="FlowStop"  FlowWorkingDir=""  UpdateType="Automatic"  ExitConditionIgnoreWaitingActivities="Yes"  Owner="lsfadmin"  Operation="Schedule" >
    <JobDef ExecutionType="lsf" Name="pm_test_flow:flow_2">
      <OnHoldForTest  Value="Keep"/>
      <JobCmdLine  Value="sleep 100"/>
      <StderrOverride  Value="No"/>
      <CreateStderr  Value="No"/>
      <CreateStdout  Value="No"/>
      <UserName  Value="lsfadmin"/>
      <StdoutOverride  Value="No"/>
      <SubmissionCmd  Value="bsub"/>
      <MinMaxCPU  Value="1,1"/>
      <StartPointForTest  Value="Keep"/>
    </JobDef>
    <JobDef ExecutionType="lsf" Name="pm_test_flow:flow_1">
      <OnHoldForTest  Value="Keep"/>
      <JobCmdLine  Value="sleep 50"/>
      <StderrOverride  Value="No"/>
      <CreateStderr  Value="No"/>
      <CreateStdout  Value="No"/>
      <UserName  Value="lsfadmin"/>
      <StdoutOverride  Value="No"/>
      <SubmissionCmd  Value="bsub"/>
      <MinMaxCPU  Value="1,1"/>
      <StartPointForTest  Value="Keep"/>
    </JobDef>
    <JobFlowDef  Name="pm_test_flow">
      <ActivityDef  Name=""  ImplementType="Job"  ImplementRefer="pm_test_flow:flow_2"  PositionX="401"  PositionY="277" Description="null" ><Events CombinationType="And"><Event GeneratorType="lsf"  IsProxy="No"  PositionY="1"  PositionX="1"  Description="" >Done(pm_test_flow:flow_1)</Event></Events><Exceptions></Exceptions></ActivityDef>
      <ActivityDef  Name=""  ImplementType="Job"  ImplementRefer="pm_test_flow:flow_1"  PositionX="199"  PositionY="161" Description="null" ><Exceptions></Exceptions></ActivityDef>
    </JobFlowDef>
    </JobFlowReq>
    ```

3. Create a flow definition with `jcommit` command as shown:

    ```pre
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jcommit pm_test_flow.xml 
    Flow <lsfadmin:test_pm_flow> is committed. Version <1.0>.
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$
    ```

4. Run the following commands to trigger the flow definition:

    ```pre
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jtrigger test_pm_flow
    Flow <lsfadmin:test_pm_flow> is triggered: Flow id <7>.[lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ 
    ```

5. Verify the status of the job by:

    ```pre
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jdefs test_pm_flow
    NAME           USER           STATUS         FLOW_IDS
    test_pm_flow   lsfadmin       OnHold         7(Done)
    [lsfadmin@-re1-mgmt-1-9e6e-001 ~]$ 
    ```

6. Run the following commands to kill/destroy the running jobs:

    ```pre
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jtrigger test_pm_flow
    Flow <lsfadmin:test_pm_flow> is triggered: Flow id <8>.
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jdefs test_pm_flow
    NAME           USER           STATUS         FLOW_IDS
    test_pm_flow   lsfadmin       OnHold         7(Done)
                                                 8(Running)
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jkill 8
    Flow <8> is killed.
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jdefs test_pm_flow
    NAME           USER           STATUS         FLOW_IDS
    test_pm_flow   lsfadmin       OnHold         7(Done)
                                                 8(Killed)
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$
    ```

7. Remove the flow definition from Process Manager, by running the commands:

    ```pre
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$ jdefs -u all
    NAME           USER           STATUS         FLOW_IDS
    test571       lsfadmin       OnHold         1(Done)
    hpc123         lsfadmin       OnHold         2(Done)
                                                 3(Done)
                                                 4(Done)
                                                 5(Done)
    [lsfadmin@test-re1-mgmt-1-9e6e-001 ~]$
    ```

For more information on Process Manager CLI commands, see [IBM Spectrum LSF Process Manager Command Reference](https://www.ibm.com/docs/en/slpm/10.2.0?topic=reference-commands).
