---
layout: post
title: Find private Teams channels in SharePoint
date: 2022-12-05
author: Hans-Carl
categories:
    - PowerShell
    - MS Teams
---

# Overview
While building some reports for SharePoint, I realized that I could not find a good explanation online on how to get the SharePoint sites connected to a Teams' private channels. Let's fix that :)

# Setup
Assume you have a MS Teams Team, that has some public channels and at least one private channel.
We will use Microsoft Graph PowerShell SDK and the `PnP.PowerShell` module to interact with SharePoint and Graph respectively.

Every MS Teams Team is based on an AzureAD group object. For our demo let us assume the group's object id is 
```powershell
$groupId = 'ceaaedd0-4a99-46f8-a08a-6cf28c254393' 
```

If you just want to find the SharePoint site URL of the team and its public channels, this is directly possible with MS Graph API.

Let us connect to MS Graph first, using `Connect-MGGraph`. Then, you can directly query the SharePoint site based to the teams group id. 

```powershell
$siteObj = Invoke-MgGraphRequest -uri "https://graph.microsoft.com/v1.0/groups/$groupId/sites/root"
if ($siteObj.webUrl) {
    "SharePoint site found at '$($siteObj.webUrl)'!"
}
``` 

A team's private channels are not part of that site. They are handled as independent SharePoint sites, but without being backed by an AzrueAD group. MS Graph seems not to offer a straightforward way to discovery these sites/channels.

SharePoint will help you here. You need to connect to your tenants admin site first. 

If your SharePoint tenant is `contoso.sharepoint.com` then your admin site will be `contoso-admin.sharepoint.com`. Let us use some regex to automate this.

```powershell
$pattern = [regex] '\.sharepoint\.com/sites/.*'
# Find the admin site URL
$adminUrl = ($siteObj.webUrl -replace $pattern) + "-admin.sharepoint.com"

# Connect
Connect-PnPOnline -Url $adminUrl
```

We will ask SharePoint for all available sites in this tenant...

```powershell
$allSites = get-PnPTenantSite
```

... and filter those with the `RelatedGroupId` property matching our group's Id. If we just want the private channels, we will further filter by those sites not having a group id.

```powershell
$privateChannels = $allSites | Where-Object { ($_.RelatedGroupId -eq $groupId) -and ($_.GroupId -eq "00000000-0000-0000-0000-000000000000") }
foreach ($channel in $privateChannels) {
    "## Channel Site found - '$($channel.Url)'"
}
```

And there you have your team's private channel's sites :)

