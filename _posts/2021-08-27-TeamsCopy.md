---
layout: post
title: AI's almost imperceptible improvement
date: 2021-08-27
author: Hans-Carl
categories:
    - MS Graph
    - PowerShell
    - MS Teams
---

# Overview
Recently I had to set up a new team in MS Teams, that included a LOT of private channels. Also, I was not the only one in charge of that team, so there where other people that had to be either owner or member of those channels. If you had the pleasure of doing this manully already - it is a lot of clicking. 

So let us automate this a bit :) As I am a fan of PowerShell and MS Graph, I will use these as my weapons of choice.

What are we trying to solve? Basically, 
- let me take a list of already existing users from one channel / the team incl. their permission (member or owner) 
- add those users to every private channel in this team. 
- make sure they have the same permission everywhere (owner/member)

For this procedure, you can use basically anythig that can talk [REST](https://de.wikipedia.org/wiki/Representational_State_Transfer). Just for quality of life, I will use my own MS Graph wrapper / PS module [MEMPSToolkit](https://github.com/hcoberdalhoff/MEMPSToolkit/). This allows me to use `Invoke-GraphRestRequest`, which abstracts some of the REST-headaches away.

In this post I assume you already managed to [authenticate](https://github.com/hcoberdalhoff/MEMPSToolkit/#authenticate-against-ms-graph) and have enough permissions. 

# Gather information
A "team" in MS Teams is based on an Office365 group, and identified by that groups object id in AzureAD. So, as a first step, we need to grab that id. You can copy it from the AzureAD- or Teams- admin portal, or you can query MS Graph:

```powershell
Invoke-GraphRestRequest -resource "/groups" | Select-Object -Property "DisplayName", "id"
```

This will give you a list of available teams and the respective id. Store the id you want in a variable. 

```powershell
$teamId = "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeeeeee"
```

(This one is obviously fake.)

Now, let us grab a list of your team's private channels.
```powershell
$channels = Invoke-GraphRestRequest -resource "/teams/$teamId/channels")  | Where-Object { $_.membershipType -eq "private" }
```

In MEMPSToolkit you could also just use `Get-TeamsChannels -teamId $teamId`. I'll stick to generic requests in this post, so you can apply this in other toolkits.

Grab a list of current members/owners of the root team. Remember, we want to apply those to all private channels.
```powershell
$members = Invoke-GraphRestRequest -resource "/teams/$teamid/members"
```

# Add users - the bulky way
We have everything now. We simply iterate over all the private channels in this team and add users. 

For every user we need to create a `#microsoft.graph.aadUserConversationMember` that will reference the users object id in AzureAD as well as specifying the desired permission (`roles`). We will take both pieces of info from the list `$members` we already collected before. 

We will output the current channel (`$channel`) and user's email (`$_.email`) just to see progress in the shell.

```powershell
$channels | ForEach-Object {
    $channelid = $_.id
    "Channel: $($_.displayName)" 
    $members | ForEach-Object {
        "User: $($_.email)"
        $body = @{
            "@odata.type"     = "#microsoft.graph.aadUserConversationMember"
            "roles"           = $_.roles
            "user@odata.bind" = "https://graph.microsoft.com/v1.0/users('$($_.userId)')"
        } | ConvertTo-Json 
        Invoke-GraphRestRequest -method "Post" -resource "/teams/$teamid/channels/$channelid/members" -body $body    
    }
}
```

If a user already is member in a channel, this will not fail - so don't worry. 

Teams will take a minute to process the changes. Result: All users in your source team/channel (`$members`) now have access to all the private channels.