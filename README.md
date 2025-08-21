# PermissionReplicationFunction
---
This repository contains an Azure Resource Manager (ARM) template and a parameters file for deploying a PowerShell-based Azure Function App with system-assigned managed identity.

## 📦 Contents

- `template.json`: ARM template defining the Function App, storage account, App Insights, and managed identity.
- `parameters.json`: Parameter values for deployment.

## 🚀 Deploy to Azure

Click the button below to deploy the Function App to your Azure subscription using the Azure Portal:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FSpektra-IS%2FPermissionReplicationFunction%2Fmain%2Ftemplate.json)

<br>
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FSpektra-IS%2FPermissionReplicationFunction%2Fmain%2Ftemplate.json" target="_blank">
    <img src="https://aka.ms/deploytoazurebutton" alt="Deploy to Azure">
</a>

> You can modify parameter values during deployment or use a custom `parameters.json` file.

## 🛠️ Prerequisites

- An active Azure subscription
- Permissions to deploy resources and assign identities
