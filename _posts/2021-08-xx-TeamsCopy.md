

Grab a list of yout team's private channels 
```powershell
$channels = Get-TeamsChannels -authToken $token -teamId $teamid | Where-Object { $_.membershipType -eq "private" }
```

Grab a list of current members
```powershell
$members = Invoke-GraphRestRequest -authToken $token -resource "/teams/$teamid/members"
```

Bulk add users to those private channels
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
        Invoke-GraphRestRequest -authToken $token -method "Post" -resource "/teams/$teamid/channels/$channelid/members" -body $body    
    }
}
```
