Grab a list of current members
```powershell
$members = Invoke-GraphRestRequest -authToken $token -resource "/teams/$teamid/members"
```
