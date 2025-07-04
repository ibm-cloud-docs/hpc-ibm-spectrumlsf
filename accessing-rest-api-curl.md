---

copyright:
  years: 2025
lastupdated: "2025-06-26"

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

# Accessing REST API calls with `curl`
{: #access-rest-api-calls-curl}

{{site.data.keyword.spectrum_full_notm}} Application Center provides standard RESTful web services for application submission, data management, job information query, job actions, and more. The LSF Application Center web service API can be integrated with many languages and methods. This example shows how to access the LSF Application Center REST API calls by using [`curl`](https://www.ibm.com/docs/en/slac/10.2.0?topic=reference-jobs){: external}.
{: shortdesc}

## Before you begin
{: #before-you-begin}

To access the LSF Application Center REST APIs from your command line, you need to establish an SSH connection:

    ```
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ServerAliveInterval=5 -o ServerAliveCountMax=1 -L 8443:10.241.0.10:8443 -L 6080:10.241.0.10:6080 -L 8444:10.241.0.10:8444 -J ubuntu@{bastion_node_ip} lsfadmin@{login_host_ip}
    ```
    {: codeblock}

Where `login_host_ip` needs to be replaced with the login node IP address that is associated with `<cluster_prefix>-login-host-0`, and `FLOATING_IP_ADDRESS` needs to be replaced with the bastion node-floating IP address. To find the management and login node IPs, see the instructions for [Gathering IP addresses](/docs/ibm-spectrum-lsf?topic=ibm-spectrum-lsf-accessing-lsf-gui#gathering-ip-addresses). The default user is `lsfadmin`.

## Connecting to LSF Application Center with `curl`
{: #connect-lsf-application-center-curl}

1. Open a new command line from your local device and run the following commands:

    1. Define or export the following variables:

        ```text
        export SSL_INSECURE=1
        export AC_HOST=localhost
        export AC_PORT=8443
        export AC_USER=lsfadmin
        export AC_PASSWORD=<mynicepassword>
        ```
        {: codeblock}

    2. Ping the cluster:

        ```text
        curl -k -X POST -H "Accept: application/json" https://$AC_HOST:$AC_PORT/platform/ws/ping
        ```
        {: codeblock}

    3. Log in to the cluster:

        ```text
        curl -k -X POST -H "Accept: application/json" -H "Content-Type: application/xml" -d "<User><name>$AC_USER</name> <pass>$AC_PASSWORD</pass> </User>" https://$AC_HOST:$AC_PORT/platform/ws/logon
        ```
        {: codeblock}

        The output of this command contains the token that you need to run commands. The token is only valid for a particular time frame.
        {: note}

        Example output:

        ```text
        {"csrftoken":"dbf6208a-0c01-4cb3-b6e1-9f75390311e5","token":"lsfadmin\"2022-05-25T20:53:12Z\"cn8FS6/FIXiVZc5vMpfPdoob9bosjk85u3lEPseqrWdX+DyWMPYkYgPOC5UJ+a4m87zyjHDq1DhjIZyYx1X47SFGRS4MRzeah94l+EpNBazKilXsG8cVuYyUgtz9M0J6\"PBNNTKLPN3JlyOpnCqI7cg=="}
        ```
        {: codeblock}

    4. After you receive your token, you need to make two changes to it. First, add `"platform_token="` at the beginning of the token, and second, replace the `\"` with `#quote#`. See the following example of an updated token:

        ```text
        export
        MYTOKEN=platform_token=lsfadmin#quote#2022-05-25T20:53:12Z#quote#cn8FS6/FIXiVZc5vMpfPdoob9bosjk85u3lEPseqrWdX+DyWMPYkYgPOC5UJ+a4m87zyjHDq1DhjIZyYx1X47SFGRS4MRzeah94l+EpNBazKilXsG8cVuYyUgtz9M0J6#quote#PBNNTKLPN3JlyOpnCqI7cg==
        ```
        {: codeblock}

    5. Get cluster information:

        ```text
        curl -k -X GET -H 'Content-Type: application/xml' -H "Cookie: $MYTOKEN" -H 'Accept:text/plain,application/xml,text/xml,multipart/mixed' -H 'Accept-Language:en-us'  https://$AC_HOST:$AC_PORT/platform/ws/clusters/local
        ```
        {: codeblock}

    6. Get the LSF Application Center version (Optional):

        ```text
        curl -k -X GET -H 'Content-Type: application/xml' -H "Cookie: $MYTOKEN" -H 'Accept:text/plain,application/xml,text/xml,multipart/mixed' -H 'Accept-Language:en-us'  https://$AC_HOST:$AC_PORT/platform/ws/version
        ```
        {: codeblock}

    6. List out the users (Optional):

        ```bash
        curl -k -X GET -H 'Content-Type: application/xml' -H "Cookie: $MYTOKEN" -H 'Accept:text/plain,application/xml,text/xml,multipart/mixed' -H 'Accept-Language:en-us'   https://$AC_HOST:$AC_PORT/platform/ws/users
        ```
        {: codeblock}

    8. List groups (    ):

        ```text
        curl -k -X GET -H 'Content-Type: application/xml' -H "Cookie: $MYTOKEN" -H 'Accept:text/plain,application/xml,text/xml,multipart/mixed' -H 'Accept-Language:en-us'   https://$AC_HOST:$AC_PORT/platform/ws/usergroups
        ```
        {: codeblock}

    9. List host details (Optional):

        ```text
        curl -k -X GET -H 'Content-Type: application/xml' -H "Cookie: $MYTOKEN" -H 'Accept:text/plain,application/xml,text/xml,multipart/mixed' -H 'Accept-Language:en-us'   https://$AC_HOST:$AC_PORT/platform/ws/hosts
        ```
        {: codeblock}

    10. List existing jobs:

        ```text
        curl -k -X GET -H 'Content-Type: application/xml' -H "Cookie: $MYTOKEN" -H 'Accept:text/plain,application/xml,text/xml,multipart/mixed' -H 'Accept-Language:en-us'   https://$AC_HOST:$AC_PORT/platform/ws/pacclient/jobs
        ```
        {: codeblock}

    11. Get detailed job information by job ID (GET) (Optional):

        ```text
        curl -k -X GET -H 'Content-Type: application/xml' -H "Cookie: $MYTOKEN" -H 'Accept:text/plain,application/xml,text/xml,multipart/mixed' -H 'Accept-Language:en-us'   https://$AC_HOST:$AC_PORT/platform/ws/jobs/<job ID>
        ```
        {: codeblock}

    12. Get basic and detailed job information by job attributes (GET) (Optional):

        ```text
        curl -k -X GET -H 'Content-Type: application/xml' -H "Cookie: $MYTOKEN" -H 'Accept:text/plain,application/xml,text/xml,multipart/mixed' -H 'Accept-Language:en-us'   https://$AC_HOST:$AC_PORT/platform/ws/jobs/fullinfo
        ```

2. Submit a job:

    ```curl
    curl -k -X POST \
    -H 'Content-Type: multipart/mixed; boundary=bqJky99mlBWa-ZuqjC53mG6EzbmlxB' \
    -H 'Accept: text/xml, application/xml' \
    -H "Cookie: $MYTOKEN" \
    -H 'Accept-Language: en-us' \
    -d '
    --bqJky99mlBWa-ZuqjC53mG6EzbmlxB
    Content-Disposition: form-data; name="AppName"
    Content-ID: <AppName>

    generic
    --bqJky99mlBWa-ZuqjC53mG6EzbmlxB
    Content-Disposition: form-data; name="data"
    Content-Type: multipart/mixed; boundary=_Part_1_701508.1145579811786
    Accept-Language: en-us
    Content-ID: <data>

    --_Part_1_701508.1145579811786
    Content-Disposition: form-data; name="COMMANDTORUN"
    Content-Type: application/xml; charset=UTF-8
    Content-Transfer-Encoding: 8bit
    Accept-Language: en-us

    <AppParam><id>COMMANDTORUN</id><value>sleep 99</value><type></type></AppParam>
    --_Part_1_701508.1145579811786--
    --bqJky99mlBWa-ZuqjC53mG6EzbmlxB--
    ' https://$AC_HOST:$AC_PORT/platform/webservice/pacclient/submitapp
    ```
    {: codeblock}
