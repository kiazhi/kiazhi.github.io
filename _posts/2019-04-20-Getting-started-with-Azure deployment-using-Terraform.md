---
layout: single
title: "Getting started with Azure deployment using Terraform"
excerpt: "Discussing on how you can obtain Terraform in your working
environment and briefly demonstrates on how to deploy resources with Terraform
configuration files."
author: Ryen Tang
date: 2019-04-20 00:00:00 +1200
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

In this post, I will discuss on how you can obtain Terraform in your working
environment and briefly demonstrates on how to create a resource group with
Terraform `*.tf` files containing those Terraform configuration language.

## Setting up Terraform CLI

Since Terraform is a multiple platform tool for multiple cloud service
providers and I have included Terraform tool deployment for macOS, Linux and
Windows.

### macOS

In this walkthrough section, I will demonstrate on how you can use `terraform`
on macOS Catalina to deploy in Azure.

```sh
# Go to your Downloads directory as your working directory
cd ~/Downloads

# Download the Terraform
curl -sSO https://releases.hashicorp.com/terraform/0.12.19/terraform_0.12.19_darwin_amd64.zip

# Unzip the downloaded compressed file
unzip terraform_0.12.19_darwin_amd64.zip

# Make a Terraform directory
mkdir -p ~/Terraform

# Move the terraform file to Terraform directory
mv terraform ~/Terraform

# Set up Terraform PATH
echo 'export PATH="/Users/$USER/Terraform:$PATH"' >> ~/.zshrc
```

Quit your current terminal and launch a new terminal window. Now you can check
the terraform version from the terminal to determine if the file path is
configured properly.

```sh
# Check the version
terraform -version
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

### Linux

In this walkthrough section, I will demonstrate on how you can use `terraform`
on linux to deploy in Azure.

```sh
# Go to your home directory as your working directory
cd ~

# Download the Terraform
curl -sSO https://releases.hashicorp.com/terraform/0.12.19/terraform_0.12.19_linux_amd64.zip

# Unzip the downloaded compressed file
unzip terraform_0.12.19_linux_amd64.zip

# Move the terraform file to Terraform directory
mv ~/terraform /usr/local/bin/terraform
```

Once you have moved the `terraform` file to `/usr/local/bin` directory, test if
you execute `terraform` by checking its version.

```sh
# Check the version
terraform -version
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

### Windows

In this walkthrough section, I will demonstrate on how you can use `terraform`
on Windows to deploy in Azure.

```powershell
# Download the Terraform
Invoke-WebRequest `
  -Uri "https://releases.hashicorp.com/terraform/0.12.19/terraform_0.12.19_windows_amd64.zip" `
  -UseBasicParsing `
  -OutFile "~\Downloads\terraform_0.12.19_windows_amd64.zip" ;

# Unzip the downloaded compressed file
Expand-Archive `
  -Path "~\Downloads\terraform_0.12.19_windows_amd64.zip" `
  -DestinationPath "~\Downloads" ;

# Make a Terraform directory
New-Item `
  -Path "$env:ProgramFiles\Terraform" `
  -ItemType "directory" ;

# Move the terraform file to Terraform directory
Move-Item `
  -Path "~\Downloads\terraform.exe" `
  -Destination "$env:ProgramFiles\Terraform\terraform.exe" ;

# Add to environment path
$env:Path += ";$env:ProgramFiles\Terraform"
```

Once you add it to the environment path, test if you execute `terraform.exe`
and checks its version.

```sh
# Check the version
terraform.exe -version
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

## Example - Creating resource group using Terraform

After obtaining `terraform`, let us try to create a resource group using
`terraform` as an example.

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
mkdir -p ~/Terraform/example1
```

Firstly, define the provider in the `main.tf` file.

```sh
echo \
'# Configure the Azure provider
provider "azurerm" {
  version = "~>1.5"
}' > ~/Terraform/example1/main.tf
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
}' > ~/Terraform/example1/resource.tf
```

Once you have defined the configuration in `main.tf` and `resource.tf` files,
execute `teeraform` with `init` argument to initialize the working directory.

```sh
# Initialize a working directory containing Terraform configuration files
terraform init \
  ~/Terraform/example1
```

Next, use the `plan` argument with the `-out` parameter to generate the
execution plan file from the working directory.

```sh
# Create an execution plan
terraform plan \
  -out ~/Terraform/example1/out.plan \
  ~/Terraform/example1
```

Finally, use the `apply` argument with the execution plan file to apply
changes in Azure.

```sh
# Apply the changes
terraform apply \
  -state ~/Terraform/example1/terraform.tfstate \
  ~/Terraform/example1/out.plan
```

Now, go to your Azure and check if the resource group has been created.

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

- [Terraform - Download](https://www.terraform.io/downloads.html)
- [Microsoft Docs - Azure Terraform Documentation](https://docs.microsoft.com/en-us/azure/terraform/)
- [Terraform CLI Docs - Terraform Commands (CLI)](https://www.terraform.io/docs/commands/index.html)
- [Terraform Docs - Configuration Language](https://www.terraform.io/docs/configuration/index.html)
- [Terraform Docs - Getting Started - Azure](https://learn.hashicorp.com/terraform/azure/intro_az)
- [Terraform Docs - Azure Provider](https://www.terraform.io/docs/providers/azurerm/index.html)

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
