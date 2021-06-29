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

You can interact with Conditional Access Policies using MS Graph. You can use the "/identity/conditionalAccess/policies" resource in MS Graph to create, read, update, delete individual policies.

There are several PowerShell-based frameworks to interact with these policies. A well known one is [DCToolBox](https://github.com/DanielChronlund/DCToolbox) by Daniel Chronlund. At also allows you to programmatically manage and update policies.

Conditional Access Policies are represented as JSON documents in MS Graph - as described [here](https://docs.microsoft.com/en-us/graph/api/resources/conditionalaccesspolicy).




Install Node.js on Windows, e.g. using WinGet
```
winget install -e --id OpenJS.NodeJS
```
If you don't run Windows, or don't use WinGet, go to [NodeJS.org](https://nodejs.org/en/download/) and download the distribution fitting your needs.


Now that we have Node.js set up, install Mermaid CLI using npm:
```
npm install -g @mermaid-js/mermaid-cli
```

