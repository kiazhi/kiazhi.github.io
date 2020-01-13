---
layout: single
title: "Getting started on Kubernetes orchestration system cluster for container in Azure"
excerpt: "How to create a Kubernetes Cluster in Azure? This is a quick
walk-through on creating a Kubernetes cluster in Azure using PowerShell or
Azure CLI."
author: Ryen Tang
header:
  image: ./assets/images/banners/Cloud-1280x200.png
date: 2018-08-30 00:00:00 +1200
toc: true
categories: 
  - Blog
  - Cloud
tags:
  - Blog
  - Cloud
  - Container
  - Kubernetes
  - Azure
  - AzureCLI
  - PowerShell
  - "Ryen Tang"
---

Since my previous blog post to address on
[How to deploy Azure Container Registry?](https://kiazhi.github.io/blog/cloud/Getting-started-on-Azure-container-registry-to-dock-your-containers/)
and
[How to deploy Azure Container instance from Azure Container Registry?](https://kiazhi.github.io/blog/cloud/Getting-started-on-creating-container-instance-from-Azure-container-registry/),
I kept being questioned on how do enterprises orchestrate their containers?

Ever heard of [Kubernetes](https://kubernetes.io/), an open-source system for
automating deployment, scaling, and management of containerized applications?
Today, I will demonstrate on how you can deploy this Kubernetes open-source
orchestration system cluster in Azure.

## What is an Azure Kubernetes Service (AKS)?

Azure Kubernetes Service (AKS) is a hosted Kubernetes service where Azure
handles the critical tasks like health monitoring and maintenance as a service.
It reduces the complexity and operational overhead of managing Kubernetes by
offloading much of that responsibility to Azure.

For more details about Azure Kubernetes Service (AKS), you can read more
[here](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes).

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

## Getting started with Azure Kubernetes Cluster (AKS) using PowerShell

This section covers how to use PowerShell commands from AzureRM.Aks module to
perform the task in creating an Azure Kubernetes Cluster.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

### Pre-requisite requirements

This section provides a list of pre-requisite requirements to deploy and
manage Kubernetes in Azure.

- [AzureRm](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-6.8.0)
  - [AzureRm.Aks](https://www.powershellgallery.com/packages/AzureRM.Aks/) (Prerelease Module)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

### Installing AzureRm.Aks PowerShell module

In this section, I will demonstrate on how to obtain `AzureRm.Aks` pre-release
module from [PowerShell Gallery](https://www.powershellgallery.com/).

> Note: Because `AzureRm.Aks` module is still in pre-release stage, you will
need an up to date `PowerShellGet` module in order to allow pre-release module
to be installed.

> Note: If you already have an up to date `PowerShellGet` module, you can skip
this `Update-Module` step.

```powershell
# Update PowerShellGet module
Update-Module `
  -Name "PowerShellGet" `
  -Force ;
```

Once you have the latest `PowerShellGet` module, you are use `Install-Module`
with the `-AllowPrerlease` parameter to install a pre-release module.

```powershell
# Install Pre-Release AzureRm.Aks module
Install-Module `
  -Name "AzureRm.Aks" `
  -AllowPrerelease ;
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

### Creating an Azure Kubernetes cluster using PowerShell

Assuming that you already have `AzureRm` and `AzureRm.Aks` module installed,
you will have to use `Login-AzureRmAccount` to sign-in to Azure and select an
Azure subcription using `Select-AzureRmSubscription` command using PowerShell.

```powershell
# Login to Azure using PowerShell
Login-AzureRmAccount ;

# Select an Azure subscription if you have more than
#  one Azure subscription with your account credential
# Eg. Select-AzureRmSubscription -SubscriptionId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Select-AzureRmSubscription `
    -SubscriptionId $AZURE_SUBSCRIPTION_ID ;
```

Next, you will obtain the Azure Container Registry (ACR) identifier with
`Get-AzureRmContainerRegistry` and create an Azure AD Service Principal account
with Reader role that associate to the Azure Container Registry (ACR) using the
`New-AzureRmADServicePrincipal` command.

```powershell
# Get the Azure Container Registry Id
# Eg. Get-AzureRmContainerRegistry `
#       -ResourceGroupName "containers" `
#       -Name "containersRegistry000" | `
#           Select-Object `
#               -Property "Id" ;
$AZURE_CONTAINER_REGISTRY_ID = (Get-AzureRmContainerRegistry `
    -ResourceGroupName "containers" `
    -Name "containersRegistry000").Id ;

# Create an Azure Kubernetes Service (AKS) service principal account
# Eg. New-AzureRmADServicePrincipal `
#       -DisplayName "sp-aks-cluster-pwsh"
#       -Role "Reader" `
#       -Scope "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/containers/providers/Microsoft.ContainerRegistry/registries/containersRegistry000" ;
New-AzureRmADServicePrincipal `
    -DisplayName "sp-aks-cluster-pwsh" `
    -Role "Reader" `
    -Scope $AZURE_CONTAINER_REGISTRY_ID ;
```

Now that you have created an Azure AD Service Principal account, you can use
`New-AzureRmAks` to create a Kubernetes cluster in Azure with the
`-ClientIdAndSecret <PSCredential>` parameter to include the Azure AD Service
Principle account credential.

> Note: If you do not have a SSH key pair generated in your
`$ENV:USERPROFILE\.ssh` folder, you use `ssh-keygen -t rsa -b 2048` command
line to generate a SSH key pair with
[OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/releases). If you are
using Windows 10 or Windows Server 1709, you can obtain it through
[Feature-on-Demand](https://blogs.msdn.microsoft.com/powershell/2017/12/15/using-the-openssh-beta-in-windows-10-fall-creators-update-and-windows-server-1709/).

```powershell
# Create Azure Kubernetes cluster
New-AzureRmAks `
    -ResourceGroupName "containers" `
    -Name "my-aks-cluster-000" `
    -NodeCount 1 `
    -ClientIdAndSecret (New-Object `
      -TypeName "System.Management.Automation.PSCredential" `
      -ArgumentList ( `
        (Get-AzureRmADServicePrincipal `
          -DisplayName "sp-aks-cluster-pwsh").ApplicationId, `
        (ConvertTo-SecureString `
          -String (Get-AzureRmADServicePrincipalCredential `
            -DisplayName "sp-aks-cluster-pwsh").KeyId `
          -AsPlainText `
          -Force ))) ;
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

### Importing Azure Kubernetes cluster configuration with kubectl tool

Finally, import the Azure Kubernetes cluster configuration with `kubectl` using
the `Import-AzureRmAksCredential` command in order to be able to use `kubectl`
command line tool to manage the Kubernetes in Azure.

```powershell
# Import and merge Kubectl config
Import-AzureRmAksCredential `
  -ResourceGroupName "containers" `
  -Name "my-aks-cluster-000" `
  -Force ;
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

## Creating an Azure Kubernetes cluster using Azure CLI

This section covers how to use Azure CLI commands to perform the task in
creating an Azure Kubernetes Cluster.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

### Pre-requisite requirements

This section provides a list of pre-requisite requirements to deploy and manage
Kubernetes in Azure.

  - [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
  - [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

### Creating an Azure Kubernetes cluster using Azure CLI

Assuming that you already have Azure CLI (`az`) installed, you will have to use
`az login` to sign-in to Azure and select an Azure subcription using
`az account set --subscription` command using Azure CLI.

```sh
# Login to Azure using interactive login prompt
az login

# Select an Azure subscription if you have more than
#  one Azure subscription with your account credential
# Eg. az account set --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
az account set --subscription $AZURE_SUBSCRIPTION_ID
```

Next, you will obtain the Azure Container Registry (ACR) identifier with
`az acr show` with `--query "id"` and create an Azure AD Service Principal
account with Reader role that associate to the Azure Container Registry (ACR)
using the `az ad sp create-for-rbac` command.

```sh
# Get the Azure Container Registry Id
az acr show --resource-group containers --name containersRegistry001 --query "id" --output tsv
# Create an Azure Kubenetes Service (AKS) service principal account
# Eg. az ad sp create-for-rbac \
#       --name sp-aks-cluster-az \
#       --role Reader \
#       --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/containers/providers/Microsoft.ContainerRegistry/registries/containersRegistry001
az ad sp create-for-rbac --name sp-aks-cluster-az --role Reader --scopes $AZURE_CONTAINER_REGISTRY_ID
```


You will get a response back with the `appId` and `password` values.

```text
{
  "appId": "6a6397da-75aa-4641-b60b-ed25d5a18d0e",
  "displayName": "sp-aks-cluster-az",
  "name": "http://sp-aks-cluster-az",
  "password": "e131829a-5a3d-455c-b9ce-f3775c7f375f",
  "tenant": "2xxb7f0e-b6b3-45dd-8t0f-857u7di224f1"
}
```

Now that you have created an Azure AD Service Principal account, you can use
`az aks create` to create a Kubernetes cluster in Azure with the
`--service-principal <appId value>` and `--client-secret <password value>`
parameters with those values to include the Azure AD Service Principle account
credential.

```sh
# Create an Azure Kubernetes Cluster
# Eg. az aks create \
#       --resource-group containers \
#       --name my-aks-cluster-001 \
#       --node-count 1 \
#       --service-principal 6a6397da-75aa-4641-b60b-ed25d5a18d0e \
#       --client-secret e131829a-5a3d-455c-b9ce-f3775c7f375f \
#       --generate-ssh-keys
az aks create --resource-group containers --name my-aks-cluster-001 --node-count 1 --service-principal 6a6397da-75aa-4641-b60b-ed25d5a18d0e --client-secret e131829a-5a3d-455c-b9ce-f3775c7f375f --generate-ssh-keys
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

### Importing Azure Kubernetes cluster configuration with kubectl tool

Finally, import the Azure Kubernetes cluster configuration with `kubectl` using
the `az aks install-cli` command in order to be able to use `kubectl` command
line tool to manage the Kubernetes in Azure.

```sh
az aks install-cli
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

## Conclusion

Once you have imported the Azure Kubernetes cluster configuration with
`kubectl`, you can start using the `kubectl` command line tool to manage the
Kubernetes cluster in Azure and test it out yourself.

It is just that simple, you now have a Kubernetes cluster as a service from
Azure to orchestrate those containers.

```sh
# Get a list of all Kubernetes resources
kubectl get all

# Get Azure Kubernetes cluster information dump
kubectl cluster-info dump
```

If you find that this information useful, feel free to bookmark this or share
it with your colleagues and friends.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

## References

- [Microsoft Docs: Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/)
- [Microsoft Docs: Quickstart: Deploy an Azure Kubernetes Service (AKS) cluster using Azure CLI](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough)
- [Kubernetes Docs: Kubectl Commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
- [Kubernetes Docs: Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>

## Related Books

<div id="amzn-assoc-ad-a6068eed-5536-4842-b4f6-903cfe50150d"></div><script async src="//z-na.amazon-adsystem.com/widgets/onejs?MarketPlace=US&adInstanceId=a6068eed-5536-4842-b4f6-903cfe50150d"></script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

<!-- kiazhi.github.io - In-Article - Text & Image Advertisement -->
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-8419393181202253"
     data-ad-slot="9347590764"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
