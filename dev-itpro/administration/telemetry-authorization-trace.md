---
title: Authorization Trace | Microsoft Docs
description: Learn about the Authorization telemetry in Business Central  
author: jswymer
ms.service: dynamics365-business-central
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.search.keywords: administration, tenant, admin, environment, sandbox, telemetry
ms.date: 04/01/2020
ms.author: jswymer
---

# Analyzing Authorization Trace Telemetry

[!INCLUDE[2019_releasewave2.md](../includes/2019_releasewave2.md)]


Authorization telemetry provides information about the authorization of users when they try to sign in to Business Central. This telemetry data can help you identify problems a user might experience when signing in. 

Authorization signals are emitted in two stages of sign-in. The first stage is the initial authorization. In this stage, the system verifies that the user account is enabled in the tenant and has the correct entitlements. The telemetry data includes:

- Success or failure of the sign-in attempt
- Reason for failure
- Type of user (such as normal, administrator, or delegated user)
- Whether the user belongs to the tenant or is an invited user

The next stage occurs after a successful authorization attempt, when trying to open the company (that is, when the CompanyOpen trigger run). The telemetry data indicates whether the company opened successfully or failed (for some reason).

## Operation: Success Authorization (Pre Open Company)

Occurs when a user has been successfully authorized.

### General dimensions

The following table explains the dimensions included in a **Success Authorization** signal.

|Dimension|Description or value||
|---------|-----|-----------|
|message|**Authorization steps prior to the open company trigger succeeded.**||
|severityLevel|**1**||

<!--
|operation_Name|**Success Authorization (Pre Open Company)**||-->

### Custom dimensions

The following table explains the custom dimensions included in a **Success Authorization** signal.

|Dimension|Description or value|
|---------|-----|
|authorizationStatus|**Succeeded**|
|aadTenantId|Specifies that Azure Active Directory (Azure AD) tenant ID used for Azure AD authentication. For on-premises, if you aren't using Azure AD authentication, this value is **common**. |
|component|**Dynamics 365 Business Central Server**.|
|componentVersion|Specifies the [!INCLUDE[prodshort](../developer/includes/prodshort.md)] version number.|
|environmentName|Specifies the name of the tenant environment. See [Managing Environments](tenant-admin-center-environments.md).|
|environmentType|Specifies the environment type for the tenant, such as **Production**, **Sandbox**, **Trial**. See [Environment Types](tenant-admin-center-environments.md#types-of-environments)|
|entitlementSetIds |Specifies the entitlements that the user has in Business Central.|
|guestUser|**true** indicates that the user is a guest user on the tenant.<br />**false** indicates the user belongs to the tenant.|
|telemetrySchemaVersion|Specifies the version of the [!INCLUDE[prodshort](../developer/includes/prodshort.md)] telemetry schema.|
|userType|Specifies whether the user is a **Delegated_admin**, **Internal_Admin**, or  **Normal user**. See [UserType](#usertype).|

### <a name="usertype"></a> UserType

|Value|Description|See more|
|-----|-----------|--------|
|Delegated_admin|Indicates that the user is a delegated administrator on the tenant. Delegated administrators are typically reserved for partners. Delegated administrator privileges are granted to users by the customer. You grant these privileges by setting up a Partner Relationship in the Microsoft Partner Center.|[Delegated Administrator Access to Business Central Online](delegated-admin.md)<br /><br />[Customers delegate administration privileges to partners](/partner-center/customers_revoke_admin_privileges#delegated-admin-privileges-in-azure-ad)|
|Internal_Admin|Indicates that the user is an internal administrator on the tenant. As an internal administrator, the user is assigned the **Global admin** role in the Microsoft 365 admin center.|[Administration as an internal administrator in Business Central](tenant-administration.md#administration-as-an-internal-administrator)<br /><br />[Assign admin roles in Microsoft 365 admin center](/office365/admin/add-users/assign-admin-roles)|
|Normal user|Indicates that the user is a normal user in the tenant, based on the license.|[Create Users According to Licenses](/dynamics365/business-central/ui-how-users-permissions)|

<!--
{"Component":"Dynamics 365 Business Central Server","Telemetry schema version":"0.2","telemetrySchemaVersion":"0.2","authorizationStatus":"Succeeded","Component version":"15.0.40073.41395","Environment type":"Production","componentVersion":"15.0.40073.41395","Environment name":"Production","environmentType":"Production","environmentName":"Production","deprecatedKeys":", AadTenantId, Environment name, Environment type, Component, Component version, Telemetry schema version","aadTenantId":"36093cb7-8b61-47a2-8f12-078ce1cbbf8b","AadTenantId":"36093cb7-8b61-47a2-8f12-078ce1cbbf8b","component":"Dynamics 365 Business Central Server","entitlementSetIds":"INTERNAL_ADMIN,DYN365_FINANCIALS_BUSINESS","guestUser":"False","userType":"INTERNAL_ADMIN"}

-->

## Operation: Failed Authorization (Pre Open Company)

Occurs when a user sign-in in has failed authorization.

### General dimensions

The following table explains the columns included in a Success Authorization trace.

|Dimension|Description or value|
|---------|-----|
|message|**Authorization steps prior to the open company trigger failed, see failureReason column for details.**|
|severityLevel|**1**|

<!--
|operation_Name|**Authorization Failed (Pre Open Company)**||

{"aadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","AadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","Component version":"15.0.40494.0","guestUser":"False","Telemetry schema version":"0.2","failureReason":"The user was successfully authenticated in Azure Active Directory but the user account is disabled in Business Central.","authorizationStatus":"Failed","component":"Dynamics 365 Business Central Server","environmentType":"Production","telemetrySchemaVersion":"0.2","Environment type":"Production","Component":"Dynamics 365 Business Central Server","Environment name":"Production","environmentName":"Production","deprecatedKeys":"AadTenantId, Environment name, Environment type, Component, Telemetry schema version","componentVersion":"15.0.40494.0"}

{"Telemetry schema version":"0.2","telemetrySchemaVersion":"0.2","authorizationStatus":"Failed","Component version":"15.0.40494.0","Environment name":"Production","componentVersion":"15.0.40494.0","Environment type":"Production","environmentName":"Production","environmentType":"Production","deprecatedKeys":"AadTenantId, Environment name, Environment type, Component, Telemetry schema version","AadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","aadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","component":"Dynamics 365 Business Central Server","Component":"Dynamics 365 Business Central Server","guestUser":"False","failureReason":"The user was successfully authenticated in Azure Active Directory but the user account is disabled in Business Central."}
-->

### Custom dimensions

|Dimension|Description or value|
|---------|-----|
|authorizationStatus|**Failed**|
|failureReason|Specifies why the sign-in failed. See [Troubleshooting failures](#authorizationfailures) section for details.|
|guestUser|**true** indicates that the user is a guest user on the tenant.<br />**false** indicates the user belongs to the tenant.|
|userType|Specifies whether the user is a **Delegated_admin**, **Internal_Admin**, or  **Normal user**. See [UserType](#usertype).|

<!--
|aadTenantId|Specifies that Azure Active Directory (Azure AD) tenant ID used for Azure AD authentication. For on-premises, if you aren't using Azure AD authentication, this value is **common**. |
|component|**Dynamics 365 Business Central Server**.|
|componentVersion|Specifies the [!INCLUDE[prodshort](../developer/includes/prodshort.md)] version number.|
|environmentName|Specifies the name of the tenant environment. See [Managing Environments](tenant-admin-center-environments.md).|
|environmentType|Specifies the environment type for the tenant, such as **Production**, **Sandbox**, **Trial**. See [Environment Types](tenant-admin-center-environments.md#types-of-environments)|

|telemetrySchemaVersion|Specifies the version of the [!INCLUDE[prod short](../developer/includes/prodshort.md)] telemetry schema. |
-->

### <a name="authorizationfailures"></a> Troubleshooting failures

#### The user was successfully authenticated in Azure Active Directory but the user account is disabled in Business Central.

This message occurs when the user has a valid account in Business Central, but the account is disabled. If you open the user account in Business Central, you'll see that the **State** field is set to **Disabled**.

*Resolution*

Enable the user account by setting the **State** field to **Enabled**. For more information, see [Create Users According to Licenses](/dynamics365/business-central/ui-how-users-permissions).

#### A user successfully authenticated in Azure Active Directory but the user does not have any entitlements in Business Central.

This message occurs when the user has an account in Business Central, but the account has not been assigned any entitlements. 

Entitlements are part of the license. Entitlements are permissions that describe which objects in Business Central a user can use, according to their Azure Active Directory role or license. For an explanation of entitlements, see [Business Central entitlements explained](https://cloudblogs.microsoft.com/dynamics365/it/2019/07/18/business-central-entitlements/))

*Resolution*

Entitlements are assigned to the user account in the Microsoft 365 admin center or Microsoft Partner Center. They are not assigned in Business Central. To assign entitlements to a user, see one of the following articles:

- From [Microsoft Office 365 admin center](https://admin.microsoft.com), see [Add users individually or in bulk to Office 365](https://aka.ms/CreateOffice365Users).

- From the Microsoft Partner Center, see [User management tasks for customer accounts](https://docs.microsoft.com/partner-center/assign-licenses-to-users).

## Operation: Authorization Succeeded (Open Company)

Occurs when the company has opened successfully.

### General dimensions

The following table explains the columns included in a Success Authorization trace.

|Dimension|Description or value||
|---------|-----|-----------|
|message|**Authorization steps in the open company trigger succeeded.**||
|severityLevel|**2**||

<!--
|operation_Name|**Authorization Succeeded (Open Company)**||

{"Telemetry schema version":"0.3","telemetrySchemaVersion":"0.3","serverExecutionTime":"00:00:00.1210560","authorizationStatus":"Success","Component version":"15.0.40494.0","componentVersion":"15.0.40494.0","Environment type":"Production","Environment name":"Production","environmentType":"Production","environmentName":"Production","deprecatedKeys":"Company name, AL Object Id, AL Object type, AL Object name, AL Stack trace, Client type, Extension name, Extension App Id, AadTenantId, Environment name, Environment type, Component, Telemetry schema version","aadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","companyName":"CRONUS USA, Inc.","sqlRowsRead":"52","sqlExecutes":"23","AadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","clientType":"WebClient","component":"Dynamics 365 Business Central Server","totalTime":"00:00:00.1210560","Component":"Dynamics 365 Business Central Server","result":"Success"}-->

### Custom dimensions

|Dimension|Description or value|
|---------|-----|
|authorizationStatus|**Success**|
|aadTenantId|Specifies that Azure Active Directory (Azure AD) tenant ID used for Azure AD authentication. For on-premises, if you aren't using Azure AD authentication, this value is **common**. |
|clientType|Specifies the type of client that executed the SQL Statement, such as **Background** or **Web**. For a list of the client types, see [ClientType Option Type](../developer/methods-auto/clienttype/clienttype-option.md).|
|companyName|Specifies the display name of the [!INCLUDE[prodshort](../developer/includes/prodshort.md)] company for which the report was run.|
|component|**Dynamics 365 Business Central Server**.|
|componentVersion|Specifies the [!INCLUDE[prodshort](../developer/includes/prodshort.md)] version number.|
|environmentName|Specifies the name of the tenant environment. See [Managing Environments](tenant-admin-center-environments.md).|
|environmentType|Specifies the environment type for the tenant, such as **Production**, **Sandbox**, **Trial**. See [Environment Types](tenant-admin-center-environments.md#types-of-environments)|
|result|**Success**|
|serverExecutionTime|Specifies the amount of time it took the server to open the company. The time has the format hh:mm:ss.sssssss.<br /><br />Doesn't apply to the **Cancellation report generation** trace.|
|sqlExecutes|Specifies the number of SQL statements that the report executed. <br /><br />Doesn't apply to the **Cancellation report generation** trace.|
|sqlRowsRead|Specifies the number of table rows that were read by the SQL statements.<br /><br />Doesn't apply to the **Cancellation report generation** trace.|
|telemetrySchemaVersion|Specifies the version of the [!INCLUDE[prodshort](../developer/includes/prodshort.md)] telemetry schema.|
|totalTime|Specifies the amount of time it took to open the company. The time has the format hh:mm:ss.sssssss.<br /><br />Doesn't apply to the **Cancellation report generation** trace.|

<!--
{"Component":"Dynamics 365 Business Central Server","Telemetry schema version":"0.3","telemetrySchemaVersion":"0.3","serverExecutionTime":"00:00:07.6884757","Component version":"16.0.11208.0","Environment type":"Production","componentVersion":"16.0.11208.0","environmentType":"Production","deprecatedKeys":"Company name, AL Object Id, AL Object type, AL Object name, AL Stack trace, Client type, Extension name, Extension App Id, Extension version, Telemetry schema version, AadTenantId, Environment name, Environment type, Component, Component version, Telemetry schema version","AadTenantId":"common","aadTenantId":"common","companyName":"CRONUS International Ltd.","clientType":"Background","authorizationStatus":"Success","totalTime":"00:00:07.6884757","component":"Dynamics 365 Business Central Server","result":"Success","sqlExecutes":"6","sqlRowsRead":"5"}
-->

## Operation: Authorization Failed (Open Company)

Occurs when a company has failed to open.  

### General dimensions

The following table explains the columns included in a Success Authorization trace.

|Dimension|Description or value|
|---------|-----|
|message|**Authorization steps in the open company trigger failed, see failureReason column for details.**|
|severityLevel|**3**|

<!--
|operation_Name|**Authorization Failed (Pre Open Company)**||


{"Telemetry schema version":"0.2","environmentName":"Production","Component version":"15.0.40494.0","AadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","aadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","status":"Failed","clientType":"WebClient","telemetrySchemaVersion":"0.2","companyName":"jsco","componentVersion":"15.0.40494.0","deprecatedKeys":"AadTenantId, Environment name, Environment type, Component, Telemetry schema version","Environment name":"Production","failureReason":"The user does not have permission to access the company.","component":"Dynamics 365 Business Central Server","environmentType":"Production","Environment type":"Production","Component":"Dynamics 365 Business Central Server"}

{"telemetrySchemaVersion":"0.2","componentVersion":"15.0.40494.0","environmentName":"Production","environmentType":"Production","Telemetry schema version":"0.2","aadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","Component version":"15.0.40494.0","component":"Dynamics 365 Business Central Server","Environment name":"Production","Environment type":"Production","companyName":"jsco","deprecatedKeys":"AadTenantId, Environment name, Environment type, Component, Telemetry schema version","AadTenantId":"8ca62103-8877-486d-88e2-9a91303abfc6","clientType":"WebClient","Component":"Dynamics 365 Business Central Server","failureReason":"The user does not have permission to access the company.","status":"Failed"}

-->

### Custom dimensions

|Dimension|Description or value|
|---------|-----|
|authorizationStatus|**Failed**|
|companyName|Specifies the name of the company that the user tried to open.|
|failureReason|Specifies why the sign-in failed. See [Troubleshooting failures](#opencompanyfailures) section for details.|


### <a name="opencompanyfailures"></a>Troubleshooting failures

#### The company name is not valid, because the name is either empty or exceeds the maximum allowed length.

This message occurs when a user tries to sign in to a company whose name exceeds the maximum allowed length.

*Resolution*

This message typically occurs when a user tries to access a specific company in Business Center by entering a URL in the browser address, for example, `https://businesscentral.dynamics.com/?company=CRONUS%20International%20Ltd.`. If the name exceeds 30 characters, then this message occurs. Make sure that the user has the proper name of the company.

<!--
###### The product license permits working with companies that have names that start with '<text>' only.

This message occurs when a user tries to open a company whose name does not start with the text that is required by the license.

*Resolution*
-->
#### The user does not have permission to access the company.

This message occurs when a user account in Business Central doesn't have the proper permissions to the company.

*Resolution*

In Business Central, open the user account and modify the permissions the user to give them access to the company. For more information, see [Assign Permissions to Users and Groups](/dynamics365/business-central/ui-define-granular-permissions).

> [!TIP]
> A good starting point is to look at the **Effective Permissions** that the user has on the company. You can do this from the user card by selecting **Effective Permissions** and setting the **Company** to the company in question.

#### The company doesn't exist.

This message occurs when a user tries to sign in to a company, but the company isn't found in Business Central.

*Resolution*

This message typically occurs when a user tries to access a specific company in Business Center by entering a URL in the browser address, for example, `https://businesscentral.dynamics.com/?company=CRONUS%20International%20Ltd.`. Make sure that the user has the proper name of the company.


<!--
###### The product license permits a maximum of {0} non-demonstration companies.

*Resolution*
-->
<!--
###### The user opened a company that does not have any applicable entitlement sets. This message will result in permission errors.

*Resolution*
-->
<!--

Which environment can the user(s) not sign into? e.g. sandbox/prod/all? 
When was the last successful login for the user(s)? Can the user(s) login now or is this an on-going issue? 
Has the user done a successful login before the issue? 
Can the user(s) sign into other services like Office or CRM? 
What role(s)/type(s) do these users have? e.g. Internal Admin, Delegated Admin, Normal, Device user? 
Which company does the user try to access, has the user access it before? 
Was "Refresh All User Groups" run recently by a SUPER user in BC? 
Are valid licenses assigned to the user(s), if applicable? What type of license was assigned (e.g. premium, essential, paid, trial etc.)? 

For delegated admins: 
Is the admin added to the tenant? (delegate admins are no listed as a user in the tenant user page in Azure) 
Verify the partnership is valid (in the customer tenant) 
Open portal.office.com and go to the admin page.  
Click "Partner relationships", then click the partner name.  
In the properties of the partner the property "Partner Relationship" should include "Admin" 
Verify the user (delegated admin) is an Admin or Helpdesk agent in the partner tenant.  
Open the Partner center  
Go to users and verify the type of the user to be an Admin or Helpdesk agent under the property "Assist your customers as". 
For device users, were they setup correctly? See Analyze device user login issues. 
Are the users "enabled" in the BC users page? (information available in the Cloud Manager)  
Are the users marked expired? Were users recently exported/imported via Excel?  
Was the user assigned the correct permissions from the users page? 
Were there any changes to the user recently? Like, permissions/licenses/roles change? Or maybe user was disabled/enabled? 
Permissions are assigned via the Users page in Business Central. 
Licenses are either assigned via the Office or Azure portal by the tenant admin. 
Roles are assigned via the Partner Center. 
The user can be enabled/disabled from the Users page in Business Central. 

-->
## See also

[Monitoring and Analyzing Telemetry](telemetry-overview.md)  
[Enabling Application Insights for Tenant Telemetry On-Premises](telemetry-enable-application-insights.md)  
[Enable Sending Telemetry to Application Insights](tenant-admin-center-telemetry.md#appinsights)  
