---
layout: post
title: Using Terraform to manage assignments in Intune
date: 2025-11-05
author: Hans-Carl
categories:
    - Terraform
    - Intune
---

# Overview

Managing Intune applications, policies, and configurations can become complex as your organization grows.

Terraform - using an appropriate provider - offers a powerful way to define and manage these configurations and their assignments as code, allowing for version control, repeatability, and automation.

This post will focus on how to manage assignments of Intune resources using Terraform. See [this post](https://hans-carl.com/2025/10/28/Terraform-Intune-overview/) for an overview of using Terraform with Intune to create configurations, compliance policies and applications.

# Prerequisites

Obviously, you will need to have Terraform or OpenTofu as well as an Entra ID tenant with Intune licenses.
