---
layout: post
title: Visualize Conditional Access Policies using PowerShell
date: 2021-06-21
author: Hans-Carl
categories:
    - PowerShell
    - AAD
    - Security
---

Conditional Access Policies are an important part of most AzureAD environments. They allow to protect apps and data, by e.g. enforcing Multi-Factor Authentication or by restricting unneccessary access.

You can interact with Conditional Access Policies using MS Graph via the "/identity/conditionalAccess/policies" resource to create, read, update, delete individual policies.

Conditional Access Policies are represented as JSON documents in MS Graph - as described at [docs.microsoft.com](https://docs.microsoft.com/en-us/graph/api/resources/conditionalaccesspolicy).

To make life simpler for myself, I wrote some functions into my MS Graph module [MEMPSToolkit](https://github.com/hcoberdalhoff/MEMPSToolkit) to interact with Conditional Access Policies.  

Assuming you have the module installed, lets authenticate interactively - this time using a Service Principal / clientId and clientSecret.

```powershell
$token = Get-AppLoginToken -tenant "contoso.com" -clientId "123..." -secretValue "!?#..."
```

You will need to grant `Policy.Read.All` permissions to be able to read Conditional Access Policies. Then you can fetch them using:

```powershell
$CAPols = Get-ConditionalAccessPolicies -authToken $token
```

This will give you an array of policies. Let us grab one of them and print the JSON. (I will truncate the ids - hope you understand.)

```powershell
$CAPols[2] | ConvertTo-Json -Depth 10

{
  "id": "74fd.....",
  "displayName": "Legacy 1 - Block legacy auth",
  "createdDateTime": "2020-11-11T08:24:36.5080147Z",
  "modifiedDateTime": "2021-06-21T10:11:58.5866907Z",
  "state": "enabled",
  "sessionControls": null,
  "conditions": {
    "userRiskLevels": [],
    "signInRiskLevels": [],
    "clientAppTypes": [
      "other"
    ],
    "platforms": null,
    "locations": null,
    "applications": {
      "includeApplications": [
        "All"
      ],
      "excludeApplications": [],
      "includeUserActions": []
    },
    "users": {
      "includeUsers": [
        "All"
      ],
      "excludeUsers": [
        "ff2a0....",
        "acf0a....",
        "9f0a0...."
      ],
      "includeGroups": [],
      "excludeGroups": [
        "4f095...."
      ],
      "includeRoles": [],
      "excludeRoles": []
    }
  },
  "grantControls": {
    "operator": "OR",
    "builtInControls": [
      "block"
    ],
    "customAuthenticationFactors": [],
    "termsOfUse": []
  }
}
```

Hm. This is hard to read, as users, groups, roles and apps are referenced by their MS Graph object ids. 

So I wrote a parser / interpreter `Write-ConditionalAccessPolicyToMermaid` to 
- resolve all object names
- pretty print the whole thing into a [mermaid](https://mermaid-js.github.io/mermaid/) diagram :D

```powershell
Write-ConditionalAccessPolicyToMermaid -pol $CAPols[2] -asHTML $true -authToken $token > ~\pol.html 
```

The function takes two boolean parameters telling it which kind of output to produce `-asHTML` and `-asMarkdown`. If you set both to `false`, it will create naked Mermaid (.mmd) output.

If you ommit `-authToken`, names will not be resolved.

The result looks like this:

![CA-Mermaid-Demo](/images/2021-06-29-condacc-mermaid.png)

The output is by default HTML, which will render Mermaid in the browser using JavaScript.

If you need to create PNG, SVG or PDF output directly, you can use Mermaid-Cli. Only problem: It requires Node.js - which not everyone has on his/her machine.

If you are interested, install Node.js on Windows, e.g. using WinGet
```
winget install -e --id OpenJS.NodeJS
```
If you don't run Windows, or don't use WinGet, go to [NodeJS.org](https://nodejs.org/en/download/) and download the distribution fitting your needs.


Now that we have Node.js set up, install Mermaid CLI using npm:
```
npm install -g @mermaid-js/mermaid-cli
```

Now you convert Mermaid (.mmd) or Markdown (.md) containing the diagrams into your desired format:

```
mmdc -i diagram.md -o diagram.png
```



