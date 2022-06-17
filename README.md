---
page_type: sample
languages:
  - csharp
products:
  - azure
  - service-connector
urlFragment: serviceconnector-webapp-signalr-dotnetcore
---


# Using Service Connecto to connect Azure WebApp and Azure SignalR Service
The repository offers the sample codes of connecting Azure SignalR service to Azure WebApp with `system managed identity`. Follow the steps below to create and verify the connection.

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Install the <a href="/cli/azure/install-azure-cli" target="_blank">Azure CLI</a> 2.18.0 or higher to provision and configure Azure resources.
- Sign in to Azure with CLI command

```azurecli
az login
```
- Visual Studio Code or Visual Studio.

## Create Azure Resources
- Provision an Azure WebApp
```azurecli
az group create -n <myResourceGroupName> -l eastus
az appservice plan create -g <myResourceGroupName> -n <myPlanName> --sku B1
az webapp create -g <myResourceGroupName> -n <myAppName> --runtime "DOTNETCORE|3.1" --plan <myPlanName>
```

- Provision a SignalR Service
```azurecli
az signalr create -g <myResourceGroupName> -n <mySignalRName> --sku Standard_S1
```


## Create an connection between Azure WebApp and SignalR

```azurecli
az webapp connection create signalr -g <myResourceGroupName> -n <myAppName> --tg <myResourceGroupName> --signalr <mySignalRName> --system-identity
```

## Deploy the project to WebApp

```azurecli
az webapp up
```

## validate result
Validate the connection by browse to your webapp `https://<myAppName>.azurewebsites.net/`.
```azurecli
az webapp browse
```

### Cleanup the resource
```azurecli
az group delete -n <myResourceGroupName> --yes
```