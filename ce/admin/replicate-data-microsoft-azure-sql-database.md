---
title: "Replicate Dynamics 365 (online) data to Azure SQL Database | MicrosoftDocs"
ms.custom: ""
ms.date: "2017-08-31"
ms.reviewer: ""
ms.service: "crm-online"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
applies_to: 
  - "Dynamics 365 (online)"
  - "Dynamics 365 Version 9.x"
ms.assetid: a70feedc-12b9-4a2d-baf0-f489cdcc177d
caps.latest.revision: 46
author: "Mattp123"
ms.author: "matp"
manager: "brycho"
---
# Replicate data to Azure SQL Database
The [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)]-[!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] is an add-on service made available on [!INCLUDE[pn_microsoft_appsource](../includes/pn-microsoft-appsource.md)] that adds the ability to replicate [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] data to a [!INCLUDE[pn_ms_azure_sql_database](../includes/pn-ms-azure-sql-database.md)] store in a customer-owned [!INCLUDE[pn_Windows_Azure](../includes/pn-windows-azure.md)] subscription. The supported target destinations are [!INCLUDE[pn_ms_azure_sql_database](../includes/pn-ms-azure-sql-database.md)] and [!INCLUDE[pn_Windows_Azure](../includes/pn-windows-azure.md)][!INCLUDE[pn_SQL_Server_short](../includes/pn-sql-server-short.md)] on [!INCLUDE[pn_Windows_Azure](../includes/pn-windows-azure.md)] virtual machines.  The [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] intelligently synchronizes the entire [!INCLUDE[pn_crm_shortest](../includes/pn-crm-shortest.md)] data initially and thereafter synchronizes on a continuous basis as changes occur (delta changes) in the [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] system. This helps enable several analytics and reporting scenarios on top of [!INCLUDE[pn_crm_shortest](../includes/pn-crm-shortest.md)] data with [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] data and analytics services and opens up new possibilities for customers and partners to build custom solutions.  
  
> [!NOTE]
>  You can use the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] with:  
>   
>  - [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)]  
> - [!INCLUDE[pn_crm_9_0_0_online](../includes/pn-crm-9-0-0-online.md)]  
  
 For information about the programmatic interface for managing configuration and administration of the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)], see [MSDN: Data Export Service](https://msdn.microsoft.com/library/mt788315.aspx).  
  
<a name="Prereq_DES"></a>   
## Prerequisites for using [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)]  
 To start using the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)], the following prerequisites are required.  
  
### Azure SQL Database service  
  
-   A customer owned [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)] subscription. This subscription must allow the volume of data that is synchronized.  
  
-   Firewall settings.  We recommend that you turn off **Allow access to Azure services** and specify the appropriate client IP addresses listed in this topic. More information: [Azure SQL database static IP addresses used by the Data Export Service](#SQLDB_IP_addresses)  
  
     Alternatively, you can turn on **Allow access to Azure services** to allow all Azure services access.  
  
     For [!INCLUDE[pn_SQL_Server_short](../includes/pn-sql-server-short.md)] on [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] VM, the “Connect to SQL Server over the Internet” option should be enabled. More information: [Azure: Connect to a SQL Server Virtual Machine on Azure (Classic Deployment)](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-classic-sql-connect/)  
  
-   The database user must have permissions at the database and schema level according to the following tables. The database user is used in the data export connection string.  
  
     Database permissions required.  
  
    |||  
    |-|-|  
    |Permission type code|Permission name|  
    |CRTB|CREATE TABLE|  
    |CRTY|CREATE TYPE|  
    |CRVW|CREATE VIEW|  
    |CRPR|CREATE PROCEDURE|  
    |ALUS|ALTER ANY USER|  
  
     Schema permissions required.  
  
    |||  
    |-|-|  
    |Permission type code|Permission name|  
    |AL|ALTER|  
    |IN|INSERT|  
    |DL|DELETE|  
    |SL|SELECT|  
    |UP|UPDATE|  
    |EX|EXECUTE|  
    |RF|REFERENCES|  
  
### Azure Key Vault service  
  
-   Customer owned Key Vault subscription, which is used to securely maintain the database connection string.  
  
-   Grant PermissionsToSecrets permission to the application with the id "b861dbcc-a7ef-4219-a005-0e4de4ea7dcf." This can be completed by running the [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)][!INCLUDE[pn_PowerShell_short](../includes/pn-powershell-short.md)] command below and is used to access the Key Vault that contains the connection string secret. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [How to set up Azure Key Vault](#SetupAzureKV)  
  
-   The Key Vault should be tagged with the [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)] organization (OrgId) and tenant ids (TenantId).  This can be completed by running the [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)][!INCLUDE[pn_PowerShell_short](../includes/pn-powershell-short.md)] command below. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [How to set up Azure Key Vault](#SetupAzureKV)  
  
### [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)]  
  
-   A [!INCLUDE[pn_crm_9_0_0_online](../includes/pn-crm-9-0-0-online.md)] or later version instance.  
  
-   The [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] solution must be installed. Get it now  from [Microsoft AppSource](https://appsource.microsoft.com/product/dynamics-365/mscrm.44f192ec-e387-436c-886c-879923d8a448).  
  
-   The entities that will be added to the Export Profile must be enabled with change tracking. To ensure a standard or custom entity can be synchronized go to **Customization** > **Customize the System**, and then click the entity. On the **General** tab make sure the **Change Tracking** option under the **Data Services** section is enabled.  
  
-   You must have the System Administrator security role in the instance of [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)].  
  
### Web browser  
 Enable pop-ups for the domain https://discovery.crmreplication.azure.net/ in your web browser. This is required for auto-sign in when you navigate to Settings > Data Export.  
  
<a name="perms"></a>   
## Services, credentials, and privileges required  
 To use the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] feature, you must have the following services, credentials, and privileges.  
  
-   A [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] subscription. Only users that are assigned the [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)] System Administrator security role can set up or make changes to an Export Profile.  
  
- [!INCLUDE[pn_Windows_Azure](../includes/pn-windows-azure.md)] subscription that includes the following services.  
  
    - [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)] or [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)][!INCLUDE[pn_SQL_Server_short](../includes/pn-sql-server-short.md)] on [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] virtual machines.  
  
    - [!INCLUDE[pn_azure_key_vault](../includes/pn-azure-key-vault.md)].  
  
> [!IMPORTANT]
>  To use the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] the [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] and [!INCLUDE[pn_azure_key_vault](../includes/pn-azure-key-vault.md)] services must operate under the same tenant and within the same [!INCLUDE[pn_microsoft_azure_active_directory](../includes/pn-microsoft-azure-active-directory.md)]. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Azure integration with Office 365](https://support.office.com/article/Azure-integration-with-Office-365-a5efce5d-9c9c-4190-b61b-fd273c1d425f)  
>   
>  The [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)] service can be in the same or a different tenant from the [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] service.  
  
<a name="shouldknowDES"></a>   
## What you should know before using the Data Export Service  
  
-   Export Profiles must be deleted and then re-created when you restore or move a [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] instance to a different country/region. To do this, delete the Export Profile in the EXPORT PROFILES view, then delete the tables and stored procedures, and then create a new profile. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [How to delete all Data Export Profile tables and stored procedures](#Delete_DEP)  
  
-   The [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] doesn’t work for [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] sandbox instances that are configured with **Enable administration mode** turned on. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Administration mode](https://technet.microsoft.com/en-us/library/dn659833.aspx)  
  
<a name="dataexportprofile"></a>   
## Export Profile  
 To export data from [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)], the [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] administrator creates an Export Profile.  Multiple profiles can be created and activated to synchronize data to different destination databases simultaneously.  
  
 The Export Profile is the core concept of  the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)]. The Export Profile gathers set up and configuration information to synchronize data with the destination database. As part of the Export Profile, the administrator provides a list of entities to be exported to the destination database. Once activated, the Export Profile starts the automatic synchronization of data. Initially, all data that corresponds to each selected entity is exported. Thereafter, only the changes to data as they occur to the entity records or metadata in [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] are synchronized continuously using a push mechanism in near real time. Therefore, you don’t need to set up a schedule to retrieve data from [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)].  
  
 Only entities that have change tracking enabled can be added to the Export Profile. Notice that, most of the standard [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)] entities which capture data are change tracking enabled. Custom entities must be explicitly enabled for change tracking before you can add them to an Export Profile. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Enable change tracking to control data synchronization](../admin/enable-change-tracking-control-data-synchronization.md)  
  
 The [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] does both metadata and data synchronization. Each entity translates into one table, and each field translates into a column in the destination database table. Table and column names use the schema name of the [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)] metadata.  
  
 Once activated, an Export Profile gathers statistics for data synchronization that helps in operational visibility and diagnostics of the data exported.  
  
### Data synchronization available with an Export Profile  
  
||||  
|-|-|-|  
|Category|Feature|Supported data types|  
|Initial Sync|Metadata - Basic Data Types|Whole Number, Floating Point Number, Decimal Number, Single Line of Text, Multi Line of Text, Date and Time data types.|  
|Initial Sync|Metadata - Advanced Data Types|Currency, PartyList, Option Set, Status, Status Reason, Lookup (including Customer and Regarding type lookup). PartyList is only available for export version 8.1 and above.|  
|Initial Sync|Data - Basic Types|All basic data types.|  
|Initial Sync|Data - Advanced Types|All advanced data types.|  
|Delta Sync|Modify Schema - Basic Types|Add or modify field change, all basic data types.|  
|Delta Sync|Modify Schema - Advanced Types|Add or modify field change, all advanced data types.|  
|Delta Sync|Modify Data - Basic Types|All basic data types.|  
|Delta Sync|Modify Data - Advanced Types|All advanced data types, such as PartyList.|  
  
<a name="createdataexportprofile"></a>   
## Create an Export Profile  
 Ensure that following requirements are met before creating an Export Profile.  
  
-   The [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] solution is installed in your [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] instance.  
  
-   Maintain the [!INCLUDE[pn-sql](../includes/pn-sql.md)] Database connection string in the Key Vault and copy the Key Vault URL to provide in the Export Profile. More information: [Azure: Get started with Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)  
  
-   The entities to be added to the Export Profile are enabled for change tracking. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Enable change tracking to control data synchronization](../admin/enable-change-tracking-control-data-synchronization.md)  
  
-   Your [!INCLUDE[pn-sql](../includes/pn-sql.md)] Database service has enough storage space to store the [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)] data.  
  
-   You are a System Administrator in the [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] instance.  
  
1.  In [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)], go to **Settings** > **Data Export**.  
  
2.  Review the notice, and click **Continue** or **Cancel** if you don't want to export data.  
  
3.  Click **New** to create a new Export Profile.  
  
4.  In the **Properties** step, enter the following information, and then click **Next** to continue without connecting to the Key Vault. Clicking **Validate** uses the Key Vault URL you provided to connect to the Key Vault.  
  
    - **Name**. Unique name of the profile. This field is mandatory.  
  
    - **Key Vault Connection URL**. Key Vault URL pointing to the connection string stored with credentials used to connect to the destination database. This field is mandatory. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [How to set up Azure Key Vault](#SetupAzureKV)  
  
        > [!IMPORTANT]
        >  The Key Vault Connection URL is case-sensitive. Enter the Key Vault Connection URL exactly as it is displayed after you run the [!INCLUDE[pn_PowerShell](../includes/pn-powershell.md)] commands in this topic.  
  
    - **Schema**.  Name for an alternative database schema. Only alphanumeric characters are valid. This field is optional. By default, dbo is the schema that is used for the destination [!INCLUDE[pn-sql](../includes/pn-sql.md)] Database.  
  
    - **Prefix**. Prefix to be used for the table names created in the destination database. This helps you easily identify the tables created for the Export Profile in the destination database. When specified, make sure that the prefix is less than 15 characters.  This field is optional and only alphanumeric characters are allowed.  
  
    - **Retry count**. The number of times a record is retried in case of a failure to insert or update in the destination table. This field is mandatory. Acceptable values are 0-20  and the default is 12.  
  
    - **Retry interval**. The number of seconds to wait before a retry in case of a failure. This field is mandatory. Acceptable values are 0-3600 and the default is 5.  
  
    - **Write Delete Log**. Optional setting for logging deleted records.  
  
 ![Properties tab in Create Export Profile dialog box](../admin/media/data-export-profile-1.PNG "Properties tab in Create Export Profile dialog box")  
  
5.  In the **Select Entities** step, select the entities that you want to export to the destination [!INCLUDE[pn-sql](../includes/pn-sql.md)] Database, and then click **Next**.  
  
 ![Select Entities tab in Create Export Profile dialog box](../admin/media/data-export-profile-2.PNG "Select Entities tab in Create Export Profile dialog box")  
  
6.  In the **Select Relationships** step, you can synchronize  the M:N (many-to-many) relationships that exist with the entities you selected in the previous step. Click **Next**.  
  
 ![Create Export Profile &#45; Manage Relationships &#45; Select Relationships](../admin/media/data-export-profile-3.PNG "Create Export Profile - Manage Relationships - Select Relationships")  
  
7.  In the **Summary** step, click **Create and Activate** to create the profile record and connect to the Key Vault, which begins the synchronization process. Otherwise, click **Create** to save the Export Profile and activate later.  
  
 ![Summary tab in Create Export Profile dialog box](../admin/media/data-export-profile-4.PNG "Summary tab in Create Export Profile dialog box")  
  
<a name="modify_export_profile"></a>   
## Modify an existing Export Profile  
 You can add or remove the entities and relationships in an existing Export Profile that you want to replicate.  
  
1.  In Dynamics 365 (online), go to **Settings** > **Data Export**.  
  
2.  In the All Data Export Profile view, select the Export Profile that you want to change.  
  
 ![Select an Export Profile](../admin/media/data-export-select-profile.png "Select an Export Profile")  
  
3.  On the Actions toolbar, click **MANAGE ENTITIES** to add or remove entities for data export. To add or remove entity relationships, click **MANAGE RELATIONSHIPS**.  
  
 ![Manage entities or entity relationships](../admin/media/dataexport-manage.PNG "Manage entities or entity relationships")  
  
4.  Select the entities or entity relationships that you want to add or remove.  
  
 ![Select the entities or entity relationships to add or remove](../admin/media/data-export-select-entities.PNG "Select the entities or entity relationships to add or remove")  
  
5.  Click **Update** to submit your changes to the Export Profile.  
  
> [!IMPORTANT]
>  When you remove an entity or entity relationship from an Export Profileit doesn't drop the corresponding table in the destination database. Before you can re-add an entity that has been removed, you must drop the corresponding table in the destination database.  To drop an entity table, see [How to delete Data Export Profile tables and stored procedures for a specific entity](#drop_entity).  
  
<a name="table_details"></a>   
## Table details for the destination Azure SQL Database  
 The [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] creates tables for both data and metadata. A table is created for each entity and M:N relationship that is synchronized.  
  
 Once an Export Profile is activated, these tables are created in the destination database.   These are system tables and will not have the SinkCreatedTime and SinkModifiedTime fields added.  
  
|Table name|Created|  
|----------------|-------------|  
|\<Prefix>_GlobalOptionsetMetadata|Upon Export Profile activation.|  
|\<Prefix>_OptionsetMetadata|Upon Export Profile activation.|  
|\<Prefix>_StateMetadata|Upon Export Profile activation.|  
|\<Prefix>_StatusMetadata|Upon Export Profile activation.|  
|\<Prefix>_TargetMetadata|Upon Export Profile activation.|  
|\<Prefix>_AttributeMetadata|Upon Export Profile activation.|  
|\<Prefix>_DeleteLog|Upon Export Profile activation when the delete log option is enabled.|  
  
<a name="resolve_issues"></a>   
## Resolving synchronization issues  
 Even after several retry attempts, record synchronization failures  may occur from database storage constraints or table locking due to long running queries. To resolve these failures you can force a resynchronization of only failed records or a  resynchronization of all records.  
  
1.  View your export profiles to look for any that have record synchronization failures. You do this by viewing the data profiles in the Synchronization area or by opening a Export Profile , such as this profile that has a contact entity record synchronization failure.  
  
 ![DataExport&#95;failed&#95;records&#95;exist](../admin/media/data-export-failed-records-exist.PNG "DataExport_failed_records_exist")  
  
2.  Examine the source of the synchronization failure and resolve it. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Error handling and monitoring](#error_handling)  
  
3.  After the problem has been resolved, resynchronize the failed records.  
  
    > [!NOTE]
    >  Failed records synchronization is a public preview feature.  
    >   
    >  - [!INCLUDE[cc_preview_features_definition](../includes/cc-preview-features-definition.md)]  
    > - [!INCLUDE[cc_preview_features_expect_changes](../includes/cc-preview-features-expect-changes.md)]  
    > - [!INCLUDE[cc_preview_features_no_MS_support](../includes/cc-preview-features-no-ms-support.md)]  
  
    1.  Sign in to your [!INCLUDE[pn_CRM_Online](../includes/pn-crm-online.md)] instance and go to **Settings** > **Data Export**.  
  
    2.  Open the Export Profile that includes record synch failures.  
  
    3.  On the Export Profile toolbar, click **RESYNC FAILED RECORDS**.  
  
    4.  Click **Ok** upon successful resynchronization of the failed records on the confirmation dialog.  
  
 ![Notification of a successful resynchronization](../admin/media/data-export-resync-success.PNG "Notification of a successful resynchronization")  
  
    5.  Verify that the Export Profile doesn’t contain failed record notifications by opening the data export profile and viewing the **Failed Notifications** counter on the **PROPERTIES & OVERVIEW** tab, which should be **0**. Click **REFRESH** on the Export Profile toolbar to make sure the **Failed Notifications** value is current.  
  
 ![Zero records failed  indication](../admin/media/data-export-failed-records-zero.PNG "Zero records failed  indication")  
  
4.  If the record synchronization failures persist after you've tried resynchronizing by following the previous steps, drop the tables, types, and stored procedures from the destination database, and then remove, and add back the entities to the Export Profile.  
  
    1.  Delete the associated database objects in the destination [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)]. For example, if you experience persistent leads entity synchronization issues, drop the leads tables, types, and stored procedures from the destination [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)]. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [How to delete Data Export Profile tables and stored procedures for a specific entity](#drop_entity)  
  
    2.  Remove the entity, such as the leads entity, from the Export Profile. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Modify an existing Export Profile](#modify_export_profile)  
  
    3.  Add the entity, such as the leads entity, back to the Export Profile and then activate the profile.  
  
<a name="error_handling"></a>   
## Error handling and monitoring  
 To view the synchronization status of an Export Profile, go to **Settings** > **Data Export** and open the Export Profile. On the **ENTITIES** tab, the synchronization status is displayed including a **Failed Records** column for records that could not be synchronized. For any failed records, a list of those records including the status reason can be downloaded by clicking **FAILED RECORDS** on the command bar.  
  
 ![Export Profile command bar &#45; Failed Records button](../admin/media/data-export-command-bar.png "Export Profile command bar - Failed Records button")  
  
 In the Export Profile you can click **PROPERTIES & OVERVIEW** to display the properties of the profile. Click **RELATIONSHIPS** to view the relationships synchronization status.  
  
<a name="view_failure_records"></a>   
### How to view detailed information about the records that failed to sync  
 Viewing the failed record logs can help you determine the cause of synchronization failures. To view failed records in the destination Azure destination database, use Azure Storage Explorer, a free standalone app that allows you to easily work with Azure Storage data. More information:  [Azure Storage Explorer](http://storageexplorer.com/).  
  
1.  In [!INCLUDE[pn_crm_shortest](../includes/pn-crm-shortest.md)], go to **Settings** > **Data Export**.  
  
2.  In the In the All Data Export Profile view, select the Export Profile that has failed notifications.  
  
 ![Failed notifications](../admin/media/data-export-failed-notifications.PNG "Failed notifications")  
  
3.  On the Actions toolbar, click **FAILED RECORDS**.  
  
 ![Failed records toolbar button](../admin/media/data-export-failed-records-toolbar.PNG "Failed records toolbar button")  
  
4.  In the Download Failed Records dialog box, click **Copy Blob URL**, and then click **Ok**.  
  
 ![Download failed records dialog box](../admin/media/dataexport-download-failed-records.PNG "Download failed records dialog box")  
  
    > [!NOTE]
    >  The blob URL is valid for up to 24 hours. If the URL exceeds the 24 hour period, repeat the steps described earlier to generate a new blob URL.  
  
5.  Start Azure Storage Explorer.  
  
6.  In Azure Storage Explorer, click **Connect to Azure Storage**.  
  
7.  Paste the URL from your clipboard in to the **Connect to Azure Storage** box, and then click **Next**.  
  
 ![Storage url](../admin/media/azure-store-url.png "Storage url")  
  
8.  On the Connection Summary page, click **Connect**.  
  
9. Azure Storage Explorer connects to the destination database. If failed records exist for the Export Profile, Azure Storage Explorer displays failed record  synchronization folders.  
  
#### Failed record synchronization folder structure and log files  
 The Failed Records Azure Blob storage URL points to a location that has the following folder structure:  
  
- **data**. This folder contains failed data notifications and the associated JSON for record data.  
  
- **metadata**. This folder contains failed metadata notifications and the associated JSON for metadata.  
  
- **failurelog**. This folder contains logs that provides information about the synchronization failure and the reason the failure occurred.  
  
- **forcerefreshfailurelog**. This folder contains errors from the last run of the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] Failed Records command used to resynchronize failed records.  
  
- **unprocessablemessages**.  This folder contains the data notifications that were not processed either due to deletion of  data or metadata and the associated JSON.  
  
 The failurelog and forcerefreshfailurelog folders are structured  *Year*\\*Month*\\*Day*\\*Hour* so that you can quickly locate the latest failures. All failure records older than 30 days  are deleted.  
  
 Here's an example log file that indicates a contact entity record synchronization failure.  
  
```Output  
Entity: contact, RecordId: 459d1d3e-7cc8-e611-80f7-5065f38bf1c1, NotificationTime: 12/28/2016 12:32:39 AM, ChangeType: Update, FailureReason: The database 'tempdb' has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions.  
The statement has been terminated.  
```  
  
<a name="recordsync_failure_reason"></a>   
#### Common reasons for record synchronization failures  
 Here are a few reasons why record synchronization failures may occur.  
  
-   Insufficient storage for the destination database.  Before you try to resynchronize the failed records, increase or free [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)] storage as appropriate. When this problem occurs, a message similar to this is recorded to the failure log.  
  
     The database 'databasename' has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions.  
  
-   Synchronization timeouts with [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)]. This can occur during the initial synchronization of a data export profile when large amounts of data are processed at one time. When this issue occurs, resynchronize the failed records. [Resolving synchronization issues](#resolve_issues)  
  
<a name="DES_best_practice"></a>   
## Best practices when using Azure SQL Database with Data Export  
  
-   To avoid synchronization errors due to resource throttling, we recommend that you have an [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)] Premium P1 or better plan when you use the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)]. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Azure SQL Database resource limits](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits) and [SQL Database Pricing](https://azure.microsoft.com/pricing/details/sql-database/)  
  
-   Set the [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)] to use read committed snapshot isolation (RCSI) for workloads running concurrently on the destination database that execute long running read queries, such as reporting and ETL jobs. This reduces the occurrence of timeout errors that can occur with the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] due to read\write conflicts.  
  
<a name="SetupAzureKV"></a>   
## How to set up Azure Key Vault  
 Run the [!INCLUDE[pn_PowerShell](../includes/pn-powershell.md)] script described here as an [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] account administrator to give permission to the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)] feature so it may access your Azure Key Vault. This script displays the key vault URL required for creating the Export Profile that is used to access the connection string.  
  
 Before running the script, replace the placeholders for the following variables.  
  
-   $subscriptionId. The Key Vault resource group you want to use. If a resource group doesn’t already exist a new one with the name you specify will be created. In this example, *ContosoResourceGroup1* is used.  
  
-   $location. Specify the location where the resource group is, or should be, located, such as *West US*.  
  
-   $connectionString. The connection string to the [!INCLUDE[pn_ms_azure_sql_database](../includes/pn-ms-azure-sql-database.md)]. You can use the ADO.NET connection string as it is displayed in your [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] dashboard.  
  
-   $organizationIdList = Comma separated list of allowed [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)] organizations, listed by organization Id (organizationId), to enable for [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)].  To find an organization's Id, in [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)] go to **Settings** > **Customizations** > **Developer Resources**. The organization Id is under **Instance Reference Information**.  
  
-   $tenantId.  Specifies the [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] Active Directory tenant Id to which the Key Vault subscription.  
  
> [!IMPORTANT]
>  An [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] subscription can have multiple [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] Active Directory tenant Ids. Make sure that you select the correct [!INCLUDE[pn_azure_shortest](../includes/pn-azure-shortest.md)] Active Directory tenant Id that is associated with the instance of [!INCLUDE[pn_microsoftcrm](../includes/pn-microsoftcrm.md)] that you will use for data export.  
  
<a name="SQLDB_IP_addresses"></a>   
## Azure SQL database static IP addresses used by the Data Export Service  
 In [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)], click **Set server firewall**, turn **Allow access to Azure services** to **OFF**, click **Add client IP**, and then add the IP addresses appropriate for the region of your [!INCLUDE[pn_Azure_SQL_Database_long](../includes/pn-azure-sql-database-long.md)]. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Azure: Configure an Azure SQL Database server-level firewall rule using the Azure Porta](https://azure.microsoft.com/documentation/articles/sql-database-configure-firewall-settings/)l  
  
|Region|IP address|  
|------------|----------------|  
|West US|40.112.139.218|  
|East US|23.96.92.86|  
|West Europe|40.68.252.224|  
|East Asia|52.175.24.148|  
|Southeast Asia|52.163.231.218|  
|Central India|52.172.191.195|  
|South India|52.172.51.15|  
|North Europe|52.169.117.212|  
|Japan West|138.91.22.196|  
|Japan East|13.73.7.177|  
|Brazil South|191.235.81.249|  
|Australia Southeast|40.115.78.163|  
|Australia East|13.73.202.160|  
|Canada Central|52.228.26.31|  
|Canada East|40.86.251.81|  
|United Kingdom South|51.140.71.166|  
|United Kingdom West|51.141.44.218|  
  
<a name="DES_knownissues"></a>   
## Known issues  
  
### Deleted records may get reinserted into entity table after a synchronization failure  
 When you recover from synchronization failures, records that had been previously deleted may get reinserted back into the originating entity table. To work around this issue when synchronization failures occur, follow these steps.  
  
1.  Create Export Profiles that are Write Delete Log enabled. Re-create existing Export Profiles that don't have Write Delete Log enabled.  
  
2.  Create and execute a SQL query for the Azure SQL destination database that searches for records in the DeleteLog table. If one or more records are found it indicates the presence of deleted records.  
  
3.  If one or more records exist in the DeleteLog table, create and run a SQL query that detects instances where the record Id for a record found in the DeleteLog table matches the record Id for a record in an *EntityName* table. When a record Id match occurs, delete the record from the *EntityName* table. For example, if a record Id in the AccountId column of the DeleteLog table matches a record Id in the AccountId column of the AccountBase entity table, delete the record from the AccountBase entity table.  
  
    > [!IMPORTANT]
    >  We recommend that you execute the SQL queries for record deletion during non-operational hours.  
  
### Entities that don't support data export  
 The entities listed here, although they support change tracking, aren't supported for data export using the [!INCLUDE[cc_Data_Export_Service](../includes/cc-data-export-service.md)].  
  
|Entity|Table Name|Work Around|  
|------------|----------------|-----------------|  
|Activity|ActivityPointerBase|Select the specific activity entities for export, such as Phone Call, Appointment, Email, and Task.|  
 
  
## Privacy notice  
[!INCLUDE[cc_privacy_data_export](../includes/cc-privacy-data-export.md)]
  
### See also  
 [AppSource: Dynamics 365 - Data Export Service](https://appsource.microsoft.com/product/dynamics-365/mscrm.44f192ec-e387-436c-886c-879923d8a448)   
 [What's new with Microsoft Dynamics 365 ‒ Data Export Service?](../admin/whats-new-with-data-export-service.md)   
 [Manage your data](../admin/manage-your-data.md)   
 [MSDN: Data Export Service](https://msdn.microsoft.com/library/mt788315.aspx)