---
layout: single
title: "Getting started on Azure container registry to dock your containers"
excerpt: "How to obtain a private container image repository in Azure? A
walk-through on being a ship captain calling to dock at a port in Azure with
full of containers."
author: Ryen Tang
header:
  image: ./assets/images/banners/Cloud-1280x200.png
date: 2018-08-07 00:00:00 +1200
toc: true
categories: 
  - Blog
  - Cloud
tags:
  - Blog
  - Cloud
  - Container
  - Azure
  - AzureCLI
  - PowerShell
  - "Ryen Tang"
---

All aboard! As we set sail onboard the ship with containers from public docker
repository towards a private container repository in Azure.

In this blog post, I will demonstrate on how you can create an Azure Container
Registry (ACR) and push your container images to your private repository in the
cloud using AzureRM PowerShell cmdlets or Azure CLI commands from the shell on
your desktop.

Of course, this demonstration also applies on the Cloud Shell in Azure portal if
you just prefer to work from a web browser.

## What is an Azure Container Registry?

An Azure Container Registry (ACR) is a managed Docker registry service based on
the open-source Docker Registry 2.0. To learn more about Azure Container
Registry, you can read more about it
[here](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro).
If you are interested on learning more about open-source Docker Registry, you
can find out more about it [here](https://docs.docker.com/registry/).

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

## Pre-requisite requirements

- An Azure Subscription (If you do not have one yet, you can start
[free](https://azure.microsoft.com/en-us/free/) to get started.)
- Command line (CLI) tools:
    - [AzureRM](https://www.powershellgallery.com/packages/AzureRM) on PowerShell, or
    - [AzureCLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

> Note: This walk-through can be performed on Cloud Shell without any
installation of the command line (CLI) tools mentioned in the pre-requisite
requirements. If you already have an Azure subscription, just launch
[Cloud Shell](https://shell.azure.com/) to get started and skip those
installation sections.

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

## Creating Azure Container Registry using PowerShell

This section covers how to use PowerShell commands from AzureRM module to
perform the task in creating an Azure Container Registry (ACR) to store our
container images.

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

### Installation of AzureRM for Windows PowerShell

If you are using Windows PowerShell, launch your Windows PowerShell console
with elevated privileges and use `Install-Module` cmdlet to request the
package from [PowerShell Gallery](https://www.powershellgallery.com/).
After the package download has completed, use the `Import-Module` cmdlet
to import the module into the current PowerShell console session.

```powershell
# Install Azure PowerShell module
Install-Module `
    -Name "AzureRm" `
    -Force ;

# Import Azure PowerShell module
Import-Module `
    -Name "AzureRm" ;
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

### Installation of AzureRM for PowerShell Core

If you are using PowerShell Core, launch your PowerShell console and use
`Install-Module` cmdlet to request the package from
[PowerShell Gallery](https://www.powershellgallery.com/).
After the package download has completed, use the `Import-Module` cmdlet
to import the module into the current PowerShell Core console session.

```powershell
# Install Azure PowerShell module
Install-Module `
    -Name "AzureRm.NetCore" `
    -Force ;

# Import Azure PowerShell module
Import-Module `
    -Name "AzureRm.NetCore" ;
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

### Login to Azure

Use `Connect-AzureRmAccount` cmdlet to prompt for your Azure sign-in credential.

```powershell
# Connect to Azure using interactive login prompt
Connect-AzureRmAccount ;
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

### Select an Azure subscription

If you have more than one Azure subscriptions, you will required to select an
Azure subscription using the `Select-AzureRmSubscription` so that any work
commencing with be targeted to the specific subscription. If you have only
Azure subscription, that will be your default subscription and you can skip
this.

> Note: Replace the `$AZURE_SUBSCRIPTION_ID` variable with your Azure
subscription id (Eg. xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx). If you are unsure
of your subscription id, use `Get-AzureRmSubscription` to list all available
Azure subscriptions.

```powershell
# Select an Azure subscription if you have more than
#  one Azure subscription with your account credential
Select-AzureRmSubscription `
    -SubscriptionId $AZURE_SUBSCRIPTION_ID ;
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

### Create a resource group for container registry

Firstly, create a resource group to contain your Azure Container Registry (ACR)
instance. If you have an existing resource group that can be used to contain
your Azure Container Registry (ACR) instance, feel free to skip this step.

> Note: For demonstration purposes, I will use `containers` as the resource
group name and `southeastasia` as the location where this resource will be
located.

> Note: For a list of Azure datacenter location, use
`Get-AzureRmLocation | Where-Object { $_.Providers -contains 'Microsoft.ContainerService' } ;`
to enumerate a list of available locations.

```powershell
# Create your container resource group for a region
#  or location
New-AzureRmResourceGroup `
    -Name "containers" `
    -Location "southeastasia" ;
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

### Create a container registry in the resource group

Now, create the Azure Container Registry (ACR) using the 
`New-AzureRMContainerRegistry` in the resource group and store the response to
a `$ContainerRegistry` variable.

> Note: For demonstration purposes, I will use `containers` as the resource
group name that we have previously created and `containerRegistry000` as the
Azure Container Registry (ACR) name.

```powershell
# Create an Azure container registry
$ContainerRegistry = New-AzureRMContainerRegistry `
    -ResourceGroupName "containers" `
    -Name "containerRegistry000" `
    -EnableAdminUser `
    -Sku "Basic" ;
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

### Login to container registry

Use `Get-AzureRmContainerRegistryCredential` cmdlet to retrieve the
`$ContainerRegistry` credential and store it to a `$Credential` variable.

Pipeline the `$Credential` variable password property value to `docker login`
with `$Credential` variable username property value for signing into Azure
Container Registry (ACR).

```powershell
# Get the Azure container registry credential
$Credential = Get-AzureRmContainerRegistryCredential `
    -Registry $ContainerRegistry ;

# Login to Azure container registry using Docker CLI
$Credential.Password | `
    docker login $ContainerRegistry.LoginServer -u $Credential.Username --password-stdin
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

### Pull a public container and push to your private container registry

To test your Azure Container Registry (ACR) is created properly, you can try
pulling a public container using `docker pull` command to your environment and
change tag of the public container repository to your private container
repository using `docker tag`.

Once you have changed the tag, push the ACR FQN tagged container using the
`docker push` command and check if docker has uploaded it successfully to your
ACR in Azure.

```powershell
# Pull a public container image from Docker Hub
#  using Docker CLI
docker pull kiazhi/nanoserver.powershell

# Construct the container tag for private Azure
#  container registry repository
$ContainerImage = "$($ContainerRegistry.LoginServer)/nanoserver.powershell:latest"

# Tag the public container image with the new container
#  tag for private Azure container registry repository
docker tag kiazhi/nanoserver.powershell $ContainerImage

# Push the tagged container to private Azure
#  container registry repository
docker push "$($ContainerRegistry.LoginServer)/nanoserver.powershell:latest"
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

## Creating Azure Container Registry using Azure CLI

This section covers how to use Azure CLI commands to perform the same task in
creating an Azure Container Registry (ACR) to store our container images.

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

### Installation of Azure CLI with Windows

If Windows PowerShell or PowerShell Core is not your favourite command line
(CLI) tool set and you would like to give Azure CLI a try, you can use
`bitadmin` command to download the Microsoft Installer (MSI) file and use
`msiexec` command to install the Azure CLI from command prompt.

```sh
:: Download Azure CLI from Command Prompt
bitsadmin /transfer wcb /priority high https://aka.ms/installazurecliwindows %USERPROFILE%\Downloads\Azure-CLI.msi

:: Install Azure CLI
msiexec /i %USERPROFILE%\Downloads\azure-cli.msi
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

### Installation of Azure CLI with Apt

For Debian or Ubuntu users, you can follow this guide on installing the Azure
CLI using `apt-get` command in Bash.

```sh
# Obtain the Microsoft signing key
curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Configure your source list based on your
#  Linux distro
AZ_REPO=$(lsb_release -cs)
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list

# Install Azure CLI
sudo apt-get install apt-transport-https && sudo apt-get update && sudo apt-get install azure-cli
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

### Installation of Azure CLI with Yum

For RedHat, Fedora or CentOS users, you can follow this guide on installing the
Azure CLI using `yum` command in Bash.

```sh
# Obtain the Microsoft signing key
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Configure your repository list based on your
#  Linux distro
sudo sh -c 'echo -e "[azure-cli]\nname=Azure CLI\nbaseurl=https://packages.microsoft.com/yumrepos/azure-cli\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'

# Install Azure CLI
sudo yum install azure-cli
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

### Installation of Azure CLI with Zypper 

For SUSE Enterprise Linux Server (SLES) or openSUSE users, you can follow this
guide on installing the Azure CLI using `zypper` command in Bash.

```sh
# Obtain the Microsoft signing key
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Configure your repository list based on your
#  Linux distro
sudo zypper addrepo --name 'Azure CLI' --check https://packages.microsoft.com/yumrepos/azure-cli azure-cli

# Install Azure CLI
sudo zypper install --from azure-cli -y azure-cli
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

### Login to Azure

Use `az login` command to prompt for your Azure sign-in credential.

```sh
# Login to Azure using interactive login prompt
az login
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

### Select an Azure subscription

If you have more than one Azure subscriptions, you will required to select an
Azure subscription using the Azure CLI so that any work commencing with be
targeted to the specific subscription. If you have only Azure subscription,
that will be your default subscription and you can skip this.

> Note: Replace the `$AZURE_SUBSCRIPTION_ID` variable with your Azure
subscription id (Eg. xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx). If you are unsure
of your subscription id, use `az account list --output table` to list all
available Azure subscriptions.

```sh
# Select an Azure subscription if you have more than
#  one Azure subscription with your account credential
az account set --subscription $AZURE_SUBSCRIPTION_ID
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

### Create a resource group for container registry

Firstly, create a resource group to contain your Azure Container Registry (ACR)
instance. If you have an existing resource group that can be used to contain
your Azure Container Registry (ACR) instance, feel free to skip this step.

> Note: For demonstration purposes, I will use `containers` as the resource
group name and `southeastasia` as the location where this resource will be
located.

> Note: For a list of Azure datacenter location, use `az acs list-location` to
enumerate a list of available locations.

```sh
# Create your container resource group for a region
#  or location
az group create --name containers --location southeastasia 
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

### Create a container registry in the resource group

Now, create the Azure Container Registry (ACR) using the 
`az acr create` in the resource group.

> Note: For demonstration purposes, I will use `containers` as the resource
group name that we have previously created and `containerRegistry001` as the
Azure Container Registry (ACR) name.

```sh
# Create an Azure container registry
az acr create --resource-group containers --name containerRegistry001 --sku Basic --admin-enabled
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

### Login to container registry

Next, you will use `az acr login` command to login to the Azure Container
Registry (ACR).

```sh
# Login to Azure container registry using Azure CLI
az acr login --name containerRegistry001
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

### List the container registry fully qualified domain name

To find out the Fully Qualified Domain Name (FQDN) of the Azure Container
Registry (ACR), use the `az acr list` command to list all the ACR FQDN that
are available in that resource group.

```sh
# List the Azure container registry 
az acr list --resource-group containers --query "[].{acrLoginServer:loginServer}" --output table
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

### Pull a public container and push to your private container registry

To test your Azure Container Registry (ACR) is created properly, you can try
pulling a public container using `docker pull` command to your environment and
change tag of the public container repository to your private container
repository using `docker tag`.

Once you have changed the tag, push the ACR FQDN tagged container using the
`docker push` command and check if docker has uploaded it successfully to your
ACR in Azure.

> Note: For demonstration purposes, you will have to replace the `$ACR_FQDN`
variable with your real ACR FQDN.

```sh
# Pull a public container image from Docker Hub
#  using Docker CLI
docker pull kiazhi/nanoserver.powershell

# Tag the public container image with private Azure
#  container registry repository
# Eg. docker tag kiazhi/nanoserver.powershell containerRegistry001.azurecr.io/nanoserver.powershell:latest
docker tag kiazhi/nanoserver.powershell $ACR_FQDN/nanoserver.powershell:latest

# Push the tagged container to private Azure container
#  registry repository
# Eg. docker push containerRegistry001.azurecr.io/nanoserver.powershell:latest
docker push $ACR_FQDN/nanoserver.powershell:latest
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

Finally, clear the deck and start pushing those containers to your new private
container repository into the public cloud in Azure using command lines in your
favourite shell. You have just created a private respository in Azure as a new
shipping destination to dock your containers.

If you need redundancy, I will highly recommend you to create the Azure
Container Registry (ACR) with a Premium SKU to enable the geo-replication of
your ACR between 2 locations.

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

## Reference

- [Microsoft Docs: About Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-intro)
- [Microsoft Docs: Azure Container Registry SKUs](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus)
- [Microsoft Docs: Quickstart: Create an Azure Container Registry using PowerShell](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-powershell)
- [Microsoft Docs: Quickstart: Create a container registry using the Azure CLI](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-azure-cli)

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
