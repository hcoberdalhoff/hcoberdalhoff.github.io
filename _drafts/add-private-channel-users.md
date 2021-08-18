Grab a list of yout team's channels 
```powershell
Get-TeamsChannels -authToken $token -teamId $teamid
```

Grab a list of current members
```powershell
$members = Invoke-GraphRestRequest -authToken $token -resource "/teams/$teamid/members"
```
