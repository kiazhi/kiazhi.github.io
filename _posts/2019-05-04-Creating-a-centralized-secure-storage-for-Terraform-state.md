---
layout: single
title: "Creating a centralized secure storage for storing Terraform state"
excerpt: "Have you deployed Azure resources using Terraform where you need to
share your its tfstate with the rest of the organization? Get an Azure storage
account for it."
author: Ryen Tang
date: 2019-05-04 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Cloud
  - Azure
  - AzureCLI
  - PowerShell
  - "Ryen Tang"
  - Terraform
---

In my previous blog post, I discussed on how you can create Azure resources
using Terraform by defining the resources configuration in a Terraform
configuration `*.tf` files.

If you have not read my previous blog post, you can check them out below:

- [Getting started with Azure deployment using Terraform](https://kiazhi.github.io/blog/Getting-started-with-Azure-deployment-using-Terraform/)

## Getting Started with a centralized Terraform state secure storage

### AzCLI

In this walkthrough section, I will demonstrate an example on how you can use
AzCLI to deploy a centralized secure storage to store all your Terraform state.

Firstly, I will start by validating all existing resource group names before
creating a new resource group in Azure using AzCLI.

```sh
export \
  resource_group_name='rg-terraform' \
  resource_location='southeastasia' \
  storage_account_name='storageterraformstate' \
  storage_account_container_name='tfstate' ;

echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
  "Validating resource group" ;

if [ "$(az group show \
  --name $resource_group_name \
  --query 'properties.provisioningState' \
  --output 'tsv')" = 'Succeeded' ] ;
then \
  
  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Existing resource group name found" ;
  
  az group show \
    --name $resource_group_name ;

else \

  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Existing resource group name not found" ;

  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Creating resource group" ;
  
  az group create \
    --name $resource_group_name \
    --location $resource_location ;

fi ;
```

Next, I will validate all existing storage account names before creating a new
storage account in Azure.

```sh
echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
  "Validating storage account" ;

if [ "$(az storage account show \
  --name $storage_account_name \
  --query 'provisioningState' \
  --output 'tsv')" = 'Succeeded' ] ;
then \
  
  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Existing storage account name found" ;
  
  az storage account show \
    --name $storage_account_name ;

else \

  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Existing storage account name not found" ;

  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Creating storage account" ;
  
  az storage account create \
    --name $storage_account_name \
    --resource-group $resource_group_name \
    --location $resource_location \
    --kind 'StorageV2' \
    --sku 'Standard_LRS' ;

fi ;
```

Since the storage account is created or exist in Azure, I will obtain the
storage account key. The storage account key will be used by Terraform later.

```sh
# Store Azure storage account key as variable
export \
  storage_account_key="$(az storage account keys list \
    --name $storage_account_name \
    --resource-group $resource_group_name  \
    --query [0].value \
    --output tsv)" ;
```

Finally, I will need to validate the existing blob container names in the
storage account and create a new blob container is it does not existing in the
storage account in Azure. The blob container will be used to contain the
Terraform `*.tfstate` state files.

```sh
echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
  "Validating storage container" ;

if [ "$(az storage container show \
  --name $storage_account_container_name \
  --account-name $storage_account_name \
  --account-key $storage_account_key \
  --query 'name' \
  --output 'tsv')" = $storage_account_container_name ] ;
then \
  
  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Existing storage container name found" ;
  
  az storage container show \
  --name $storage_account_container_name \
  --account-name $storage_account_name \
  --account-key $storage_account_key ;

else \

  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Existing storage container name not found" ;

  echo "# $(date '+%Y-%m-%d %H:%M:%S %:z') -" \
    "Creating storage container" ;
  
  az storage container create \
    --name $storage_account_container_name \
    --account-name $storage_account_name \
    --account-key $storage_account_key ;

fi ;
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

### PowerShell

In this walkthrough section, I will demonstrate an example on how you can use
PowerShell `Az` module to deploy a centralized secure storage to store all your
Terraform state.

Firstly, I will start by validating all existing resource group names before
creating a new resource group in Azure using PowerShell.

```powershell
$ResourceGroupName = 'rg-terraform' ;
$ResourceLocation = 'southeastasia' ;
$StorageAccountName = 'storageterraformstate' ;
$StorageAccountContainerName = 'tfstate' ;

Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Validating resource group") ;

if( `
  $(Get-AzResourceGroup `
  -Name $ResourceGroupName).ProvisioningState -eq 'Succeeded' ) `
{ `

  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Existing resource group name found") ;

  Get-AzResourceGroup `
    -Name $ResourceGroupName ;
  
} `
else `
{ `

  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Existing resource group name not found") ;
  
  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Creating resource group") ;

  New-AzResourceGroup `
    -Name $ResourceGroupName `
    -Location $ResourceLocation ;

} ;
```

Next, I will validate all existing storage account names before creating a new
storage account in Azure.

```powershell
Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Validating storage account") ;

if( `
  $(Get-AzStorageAccount `
    -Name $StorageAccountName `
    -ResourceGroupName $ResourceGroupName).ProvisioningState -eq 'Succeeded' ) `
{ `
  
  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Existing storage account name found") ;
  
  Get-AzStorageAccount `
    -Name $StorageAccountName `
    -ResourceGroupName $ResourceGroupName ;

} `
else `
{ `

  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Existing storage account name not found") ;

  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Creating storage account") ;
  
  New-AzStorageAccount `
    -Name $StorageAccountName `
    -ResourceGroupName $ResourceGroupName `
    -Location $ResourceLocation `
    -Kind 'StorageV2' `
    -SkuName 'Standard_LRS' ;

} ;
```

Since the storage account is created or exist in Azure, I will obtain the
storage account key. The storage account key will be used by Terraform later.

```powershell
# Store Azure storage account key as variable
$StorageAccountKey = (Get-AzStorageAccountKey `
  -Name $StorageAccountName `
  -ResourceGroupName $ResourceGroupName).Value[0] ;
```

Finally, I will need to validate the existing blob container names in the
storage account and create a new blob container is it does not existing in the
storage account in Azure. The blob container will be used to contain the
Terraform `*.tfstate` state files.

```powershell
Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Validating storage container") ;

if( `
  $(Get-AzStorageContainer `
    -Name $StorageAccountContainerName `
    -Context $(Get-AzStorageAccount `
      -Name $StorageAccountName `
      -ResourceGroupName $ResourceGroupName).Context).Name -eq $StorageAccountContainerName ) `
{ `

  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Existing storage container name found") ;
  
  Get-AzStorageContainer `
    -Name $StorageAccountContainerName `
    -Context $(Get-AzStorageAccount `
      -Name $StorageAccountName `
      -ResourceGroupName $ResourceGroupName).Context ;
  
} `
else `
{ `

  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Existing storage container name not found") ;

  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Creating storage container") ;
  
  New-AzStorageContainer `
    -Name $StorageAccountContainerName `
    -Context $(Get-AzStorageAccount `
      -Name $StorageAccountName `
      -ResourceGroupName $ResourceGroupName).Context ;

} ;
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

## Example - Creating resource group using Terraform with centralized secure storage

Assuming that you already have `terraform` in your environment, let us begin
creating a resource group using `terraform` as an example with the Terraform
`*.tfstate` state file stored in the centralized secure storage in Azure
instead of your local working directory.

If you do not have it, you can refer to my previous blog post on how to obtain
it.

> Note:
>
> Depending on whether if you are using AzCLI or PowerShell, you will need to
> authenticate to Azure.
> ```sh
> # Login to Azure using AzCLI
> az login
> ```
> or
> ```powershell
> # Login to Azure using PowerShell
> Connect-AzAccount
> ````
> Once you have authenticated, proceed with the steps below with your preferred
> console or terminal.

```sh
# Create a folder name for the terraform example
mkdir -p ~/Terraform/example2
```

> Note:
>
> Depending on which console or terminal that you are using, you may need to
> replace the `\` line break for Shell with `` ` `` line break for PowerShell.

Firstly, define the provider in the `main.tf` file.

```sh
echo \
'# Configure the Azure provider
provider "azurerm" {
  version = "~>1.5"
}

terraform {
  backend "azurerm" {}
}' > ~/Terraform/example2/main.tf
```

Secondly, define the resource in the `resource.tf` file.

```sh
echo \
'# Create a new resource group
resource "azurerm_resource_group" "rg" {
  name     = "terraform-resource-group"
  location = "southeastasia"
  tags      = {
    Environment    = "Development"
    DeploymentType = "Terraform"
  }
}' > ~/Terraform/example2/resource.tf
```

Once you have defined the configuration in `main.tf` and `resource.tf` files,
execute `teeraform` with `init` argument to initialize the working directory.

```sh
# Initialize a working directory containing Terraform configuration files
terraform init \
  -backend-config="storage_account_name=$StorageAccountName" \
  -backend-config="container_name=$StorageAccountContainerName" \
  -backend-config="access_key=$StorageAccountKey" \
  -backend-config="key=azure.terraform.tfstate" \
  ~/Terraform/example2
```

Next, use the `plan` argument with the `-out` parameter to generate the
execution plan file from the working directory.

```sh
# Create an execution plan
terraform plan \
  -out ~/Terraform/example2/out.plan \
  ~/Terraform/example2
```

Finally, use the `apply` argument with the execution plan file to apply
changes in Azure.

```sh
# Apply the changes
terraform apply \
  -state ~/Terraform/example2/terraform.tfstate \
  ~/Terraform/example2/out.plan
```

Now, go to your Azure and check if the resource group has been created. You can
also find a Terraform `*.tfstate` file created in the storage account blob
container as the centralized secure storage location.

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

- [Microsoft - Azure Terraform Documentation](https://docs.microsoft.com/en-us/azure/terraform/)
- [Terraform - Terraform Commands (CLI)](https://www.terraform.io/docs/commands/index.html)
- [Terraform - Configuration Language](https://www.terraform.io/docs/configuration/index.html)
- [Terraform - Terraform State](https://learn.hashicorp.com/terraform/azure/build_az#terraform-state)
- [Getting started with Azure deployment using Terraform](https://kiazhi.github.io/blog/Getting-started-with-Azure-deployment-using-Terraform/)

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

<div id="amzn-assoc-ad-f3a340a5-ce4d-4b4c-b409-c4c202ba7ffe"></div><script async src="//z-na.amazon-adsystem.com/widgets/onejs?MarketPlace=US&adInstanceId=f3a340a5-ce4d-4b4c-b409-c4c202ba7ffe"></script>

<a target="_blank" href="https://www.amazon.sg/b?_encoding=UTF8&tag=kiazhi-22&linkCode=ur2&linkId=d879883586c65cbd98b7d21ddc7a2dce&camp=247&creative=1211&node=6314388051">t</a><img src="//ir-sg.amazon-adsystem.com/e/ir?t=kiazhi-22&l=ur2&o=66" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

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
