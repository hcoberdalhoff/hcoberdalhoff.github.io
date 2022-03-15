---
layout: post
title: AzureAD App Registrations vs. Enterprise Apps
date: 2022-01-20
author: Hans-Carl
categories:
    - MS Graph
    - PowerShell
    - AzureAD
---
# Overview

Very often when discussing permissions for applications in AzureAD I encounter confusion about what an application registration is vs. an enterprise application.

Of course you can read a similar (and more in-depth article) from Microsoft [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals).

So what are two things? I'll talk about the confusion later.

## Application Registration 
An **Application registration** is another name for an "application object" in AzureAD. Basically (as the name implies) - you create a new, globally unique application definition. 

You can kind of compare this to a template or manifest for your application. This template represents how your application will integrate into an AzureAD tenant. 

If you allow your App to be used in other AzureAD tenants this is called an "Multitenant App". If you only allow the App to be used in your own tenant then it is called a "Single Tenant App".

An App Registration defines aspects like:
- Authentication schemes your application supports
- Permissions that need to be granted for the application to work 
- Name, description, icon and other attributes of your application
- If usage in other tenants is possible

As this definition might change with your application's needs, Azure Portal offers you to interactively modify permissions etc. This is why admins very often rely on an application registration to integrate a third party app; but more on this later.

Another way to understand it is to thing of "publishing" your app to the world through an App Registration.

![Application Registration Example](/images/2022-03-14-AppReg-Sample.png)

## Enterprise Application
An **Enterprise Application** is another name for a "service principal". A "service principal" is an anchor of an application in your AzureAD tenant. Specifically it can be a reference to an application registration.  

There are multiple kinds of Enterprise Apps. This post focusses on Enterprise Apps that allow you to use an application that is defined by an App Registration in your own or another tenant. Another example are "Managed Identities" which are also service principals but are not represented by an App Registration.

In contrast to an App Registration an Enterprise App is an actual identity. That means for example
- It can interact with data and services in your tenant
- You can authenticate against the app and use it.

As the application is defined by an App Registration you are not really supposed to change the Enterprise App's metadata, permissions etc. Azure Portal thus does not offer interactive means to modify an Enterprise App other than deciding who owns it and who can use it.

![Enterprise App Example](/images/2022-03-14-EntApp-sample.png)

## How do they work together?

When you create an App Registration, AzureAD automatically creates an Enterprise App to go along with it in your tenant. You can see a reference to the Enterprise App in the App Registration's properties.

Both objects share the same "Application ID" to identify which App Registration your Enterprise App anchors.

![Application Registration Properties](/images/2022-03-14-AppReg-properties.png)

And here is the corresponding Enterprise App which was created with the App Registration. Have a look at the Application ID.

![Enterprise Application Properties](/images/2022-03-14-EntApp-properties.png)

## Permissions on App Registrations

Azure Portal makes it extremely simple to create and use an Application Registration to the point that it hides the moving parts. This becomes apparent when working with permissions on an App Registration.

Let us assign some MS Graph permissions to our App Registration to see what happens.

![Ungranted App Registration Permissions](/images/2022-03-15-AppReg-Permissions-ungranted.png)

If you chose permissions that require an admin consent (i.e. they operate on the users own stuff) you will see a little orange exclamation mark telling you permission has not yet been granted.

If you switch over to the Enterprise App's permissions, you will see... nothing. No permissions are present right now. 

![Enterprise App with no granted permissions](/images/2022-03-15-EntApp-No-Permissions.png)

When you decide to click the "Grant admin consent" button either on the App Registration or the Enterprise App the permissions laid out in the App Registration will be requested for to the Enterprise App. 

This is basically the same process any application has to go through to interact with data and services in your tenant. This is the job of an Enterprise App.

![Granting Permissions](/images/2022-03-15-EntApp-Granting.png)

As a result the permissions are then available for the Enterprise App.

![Enterprise App with granted permissions](/images/2022-03-15-EntApp-Permissions.png)

Also, the App registration will now show fully applied permissions.

![App Registration with granted permissions](/images/2022-03-15-AppReg-Permissions.png)

If you only use the App Registration UI in Azure Portal it is easy to miss that an Enterprise App is involved in the process. But it is crucially important - The App Registration will never act as an application in your tenant  - this is done by the Enterprise App of the same name. If you assign AzureAD roles to your app you actually assign the role to the Enterprise App.

if you use an App Registration to enable your code to interact with users and data, you actually prepare an Enterprise App that will do the job for you.

## Sidenotes


### App Permissions
The difference between App Registrations and Enterprise Apps is also one explanation why "delegated" vs "app" permissions can be a hot topic. 

You define app credentials and certificates on the App Registration and they become usable in every tenant that uses your app if "app permissions" are given.

![App Registrations Credentials](/images/2022-03-15-AppReg-Secrets.png)

### Managed Identities and Azure Portal

As mentioned earlier, Managed Identities are also service principals. As such, they can be found in the Enterprise Apps shard in Azure Portal. 

But Azure Portal does not offer any way to assign API Permissions, like for MS Graph Permissions to managed identities. You can easily assign permissions on Azure Resources like Subscriptions or Resource Groups. Also assigning AzureAD roles is easily possible.

If you want to assign API permissions you will need to use code or a script. Here is a short example using the PowerShell MS Graph Toolkit that might help you:

```powershell
#Requires -modules Microsoft.Graph.Applications,Microsoft.Graph.Authentication

param(
     # Please give the managed identity's or enterprise app's object id.  
    [Parameter(Mandatory = $true)]
    [string] $enterpriseAppObjId,
    [string] $permissionsTemplate = "permissions.json",
    [switch] $disconnectAfterExecution = $false
)

## Authenticate as admin (delegated). 
## Will sign you in using your browser and ask for granting permissions if needed.
Select-MgProfile -Name "v1.0"
Connect-MgGraph -Scopes "AppRoleAssignment.ReadWrite.All,Application.Read.All"

## Read the permission template
$permissions = Get-Content -Path $permissionsTemplate | ConvertFrom-Json

"## Set permissions on the enterprise app/mgd identity"
$permissions | ForEach-Object {
    $appId = $_.Id

    ## Get Service Principal from Application:
    $resourceApp = Get-MgServicePrincipal -Filter "AppId eq '$appId'" 
    $resourceId = $resourceApp.Id

    ## Get all AppRoles. Create a hashtable giving "Claim -> AppRoleId"
    $appRoles = @{}
    (Get-MgServicePrincipal -ServicePrincipalId $resourceId).AppRoles | ForEach-Object {
        $appRoles.add($_.Value, $_.Id)
    }

    ## Apply each permission to the Ent App/Mgd. Identity
    $_.AppRoleAssignments | ForEach-Object {
        New-MgServicePrincipalAppRoleAssignment -ServicePrincipalId $enterpriseAppObjId -AppRoleId $appRoles[$_] -ResourceId $resourceId -PrincipalId $enterpriseAppObjId -ErrorAction SilentlyContinue
    }
}

if ($disconnectAfterExecution) {
    Disconnect-MgGraph
}
```

This script will read from a JSON definition where you tell which API permissions to assign:

```json
[
    {
        "Name": "Microsoft Graph",
        "Id": "00000003-0000-0000-c000-000000000000",
        "AppRoleAssignments": [
            "AuditLog.Read.All",
            "Device.Read.All",
            "Reports.Read.All"
        ]
    }
]
```