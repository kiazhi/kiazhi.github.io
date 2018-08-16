---
layout: single
title: "Getting started on creating container instance from Azure container registry"
excerpt: "How to create container from a private container image repository in
Azure? A walk-through on docking your Angular on nanoserver container in Azure."
author: Ryen Tang
header:
  image: ./assets/images/banners/Cloud-1280x200.png
date: 2018-08-16 00:00:00 +1200
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

Since my previous blog post on
[Getting Started on Azure Container Registry to dock your containers](https://kiazhi.github.io/blog/cloud/Getting-started-on-Azure-container-registry-to-dock-your-containers)
for Azure Container Services (ACS), I will continue with it by demonstrating on
how you can create a container instance from the container image in Azure
Container Registry (ACR).

For this blog post, I will demonstrate on how you can create a Windows Nano
Server with Angular and NodeJS container image for Azure Container Registry
(ACR), and create a container instance using that image in Azure.

## Deploying a Container in Azure using PowerShell

This section covers how to use PowerShell commands from AzureRM module to
perform the task in creating a container image and use that image to create a
container instance in Azure.

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

### Create an Angular container Dockerfile

Firstly, I will use `New-Item` to create a directory that will store my
Dockerfile. Once the directory is created, I will create the Dockerfile that
defines Windows Nano Server with Angular and NodeJS container image using
`Set-Content`.

```powershell
# Create a working directory
New-Item `
    -Path "C:\" `
    -Name "container" `
    -ItemType "directory" ;

# Create a Dockerfile in the working directory
@"
FROM kiazhi/nanoserver.nodejs:latest AS build

RUN npm install -g @angular/cli

RUN mkdir web

WORKDIR /web

RUN ng new site

WORKDIR /web/site

ENTRYPOINT ["cmd", "/c", "ng", "serve", "--host", "0.0.0.0", "--port", "80"]
"@ | Set-Content `
    -Path ".\container\Dockerfile" ;
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

### Build the Angular container using Dockerfile

Change your current location to your working directory location using
`Set-Location` and use `docker build` to build the container image from the
configuration defined in the Dockerfile.

```powershell
# Change your current working directory to Dockerfile location
Set-Location `
    -Path "C:\container" ;

# Build the container using Docker CLI
docker build -t nanoserver.angular:latest -f "Dockerfile" .
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

### Push the Angular container to Azure Container Registry (ACR) repository

Once the container image is built from the Dockerfile, use
`Connect-AzureRmAccount` to sign-in into Azure and select an Azure subscription
using `Select-AzureRmSubscription` if required.

Get the Azure Container Registry properties using the
`Get-AzureRmContainerRegistry` and obtain the Azure Container Registry
credential with `Get-AzureRmContainerRegistryCredential` to sign-in Azure
Container Registry using Docker CLI.

Modify the Angular container tag to start with an Azure Container Registry
(ACR) Fully Qualified Domain Name (FQDN) tag and push the newly tagged
container image to Azure.

> Note: Replace the `$AZURE_SUBSCRIPTION_ID` variable with your Azure
subscription id (Eg. xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx). If you are unsure
of your subscription id, use `Get-AzureRmSubscription` to list all available
Azure subscriptions.

```powershell
# Connect to Azure using interactive login prompt
Connect-AzureRmAccount ;

# Select an Azure subscription if you have more than
#  one Azure subscription with your account credential
# Eg. Select-AzureRmSubscription -SubscriptionId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Select-AzureRmSubscription `
    -SubscriptionId $AZURE_SUBSCRIPTION_ID ;

# Get the Azure container registry
$ContainerRegistry = Get-AzureRmContainerRegistry `
    -ResourceGroupName "containers" `
    -Name "containerRegistry000" ;

# Get the Azure container registry credential
$Credential = Get-AzureRmContainerRegistryCredential `
    -Registry $ContainerRegistry ;

# Login to Azure container registry using Docker CLI
$Credential.Password | `
    docker login $ContainerRegistry.LoginServer -u $Credential.Username --password-stdin

# Tag the container in local repository to your private cloud repository using
#  Docker CLI
# Eg. docker tag nanoserver.angular:latest containerRegistry000.azurecr.io/nanoserver.angular:latest
docker tag nanoserver.angular:latest $ACR_FQDN/nanoserver.angular:latest

# Push the container to your private cloud repository using Docker CLI
# Eg. docker push containerRegistry000.azurecr.io/nanoserver.angular:latest
docker push $ACR_FQDN/nanoserver.angular:latest
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

### Create the Angular container instance in Azure

Once the container image is pushed to Azure Container Registry (ACR), use the
`New-AzureRmContainerGroup` to create a container instance in Azure.

> Note: If the container is a Windows container, you will need to use the
`-OsType` parameter to specify `Windows`.

> Note: The `-DnsNameLabel` parameter will accept a `String` value to construct
a public accessible domain name in such 
`$DNS_NAME_LABEL.$AZURE_REGION_NAME.azurecontainer.io` format.

```powershell
# Create the Angular container in Azure Container Services (ACS)
New-AzureRmContainerGroup `
    -ResourceGroupName "containers" `
    -Name "nanoserver-angular" `
    -Image "$ACR_FQDN/nanoserver.angular:latest" `
    -OsType "Windows" `
    -DnsNameLabel "angular-on-nanoserver" `
    -Port @(80) `
    -Command "cmd /c ng serve --host 0.0.0.0 --port 80 --publicHost angular-on-nanoserver.southeastasia.azurecontainer.io" `
    -RegistryCredential (New-Object `
        -TypeName "System.Management.Automation.PSCredential" `
        -ArgumentList `
            $Credential.Username, `
            (ConvertTo-SecureString `
                -String $Credential.Password `
                -AsPlainText `
                -Force)) ;
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

## Deploying a Container in Azure using Azure CLI

This section covers how to use Azure CLI commands to perform the same task in
creating a container image and use that image to create a container instance in
Azure.

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

### Create an Angular container Dockerfile

Firstly, I will use `mkdir` to create a directory that will store my
Dockerfile. Once the directory is created, I will create the Dockerfile that
defines Windows Nano Server with Angular and NodeJS container image using
`>` redirection operator.

```sh
# Create a working directory
mkdir ~/container

# Create a Dockerfile in the working directory
echo 'FROM kiazhi/nanoserver.nodejs:latest AS build

RUN npm install -g @angular/cli

RUN mkdir web

WORKDIR /web

RUN ng new site

WORKDIR /web/site

ENTRYPOINT ["cmd", "/c", "ng", "serve", "--host", "0.0.0.0", "--port", "80"]' > ~/container/Dockerfile
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

### Build the Angular container using Dockerfile

Change your current location to your working directory location using
`cd` and use `docker build` to build the container image from the
configuration defined in the Dockerfile.

```powershell
# Change your current working directory to Dockerfile location
cd ~\container

# Build the container using Docker CLI
docker build -t nanoserver.angular:latest -f "Dockerfile" .
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

### Push the Angular container to Azure Container Registry (ACR) repository

Once the container image is built from the Dockerfile, use
`az login` to sign-in into Azure and select an Azure subscription
using `az account set --subscription` if required.

Get the Azure Container Registry name using the `az acr list --output table`
and use `az acr login --name` with the Azure Container Registry name to
sign-in Azure Container Registry.

Modify the Angular container tag to start with an Azure Container Registry
(ACR) Fully Qualified Domain Name (FQDN) tag and push the newly tagged
container image to Azure.

> Note: Replace the `$AZURE_SUBSCRIPTION_ID` variable with your Azure
subscription id (Eg. xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx). If you are unsure
of your subscription id, use `Get-AzureRmSubscription` to list all available
Azure subscriptions.

```sh
# Login to Azure using interactive login prompt
az login

# Select an Azure subscription if you have more than
#  one Azure subscription with your account credential
# Eg. az account set --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
az account set --subscription $AZURE_SUBSCRIPTION_ID

# Login to Azure container registry using Azure CLI
az acr login --name containerRegistry001

# Tag the container in local repository to your private cloud repository
docker tag nanoserver.angular:latest containerRegistry001.azurecr.io/nanoserver.angular:latest

# Push the container to your private cloud repository
docker push containerRegistry001.azurecr.io/nanoserver.angular:latest
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

### Create the Angular container instance in Azure

Once the container image is pushed to Azure Container Registry (ACR), use the
`az container create` to create a container instance in Azure.

> Note: If the container is a Windows container, you will need to use the
`--os-type` parameter to specify `Windows`.

> Note: The `--dns-name-label` parameter will accept a `String` value to
construct a public accessible domain name in such 
`$DNS_NAME_LABEL.$AZURE_REGION_NAME.azurecontainer.io` format.

> Note: The `--registry-username` parameter will be the Azure Container
Registry (ACR) name and `--registry-password` will the value that you can
obtain using the
`az acr credential show --name containerRegistry001 --query passwords[0].value --output tsv`
command.

```sh
# Create the Angular container in Azure Container Services (ACS)
az container create --resource-group containers --name nanoserver-angular --image containerRegistry001.azurecr.io/nanoserver.angular:latest --os-type Windows --dns-name-label angular-on-nanoserver --ports 80 --command-line "cmd /c ng serve --host 0.0.0.0 --port 80 --publicHost angular-on-nanoserver.southeastasia.azurecontainer.io" --registry-username containerRegistry001 --registry-password pLP1x5fiONjvj8iVItPVA2q1m46ws8/Z
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

Finally, launch your web browser and navigate to the
`http://$DNS_NAME_LABEL.$AZURE_REGION_NAME.azurecontainer.io/` url to view the
default Angular web page serves to you from a container instance hosted in
Azure running with NodeJS on top of Windows Nano Server operating system.

Isn't it just amazing? That is all, folks.

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

- [Azure Container Instances Documentation](https://docs.microsoft.com/en-us/azure/container-instances/)
- [Quickstart: Create your first container in Azure Container Instances using PowerShell](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-powershell)
- [Quickstart: Create your first container in Azure Container Instances using Azure CLI](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-quickstart)
- [PowerShell - New-AzureRmContainerGroup](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerinstance/new-azurermcontainergroup)
- [Azure CLI - az container create](https://docs.microsoft.com/en-us/cli/azure/container#az-container-create)

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