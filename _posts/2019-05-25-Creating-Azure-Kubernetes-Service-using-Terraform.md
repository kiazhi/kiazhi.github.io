---
layout: single
title: "Creating Azure Kubernetes Service using Terraform"
excerpt: "Do you need Azure Kubernetes Service (AKS) to orchestrate your
containers? This is how I use Terraform to quickly deploy it in code from my
macOS terminal."
author: Ryen Tang
date: 2019-05-25 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Cloud
  - Azure
  - AzureCLI
  - "Ryen Tang"
  - Terraform
  - AKS
---

Being tasked to have an environment that can orchestrate my containers in my
development subscription, I have decided to spin up an Azure Kubernetes Service
(AKS) using Terraform to this subscription from my macOS terminal and get it up
and running as soon as possible.

Why did I choose Terraform instead of using the Azure Portal? It is because I
will need to repeat the same deployment in production subscription later. And
it can also ensure the configuration state of the resources are validated and
managed.

Therefore in this blog post, I am sharing my adventure on deploying Azure
Kubernetes Service (AKS) using Terraform with the community in this step by
step walkthrough below.


## Create an Azure Kubernetes Service (AKS) Service Principal account

Before you start, you will need have a Service Principal account for this
deployment. The Service Principal account need to be a Contributor role so that
Kubernetes can create the Azure Load Balancer for the network.

You can create the Service Principal account using
[`az ad sp`](https://docs.microsoft.com/en-us/cli/azure/ad/sp) command with
AzCLI or
[`New-AzADServicePrincipal`](https://docs.microsoft.com/en-us/powershell/module/Az.Resources/New-AzADServicePrincipal)
command with PowerShell. Once created, copy the AppId and Password values, and
input them as the `client_id` and `client_secret` default values in
`variables.tf` Terraform file.

For more knowledge on how to create a Service Principal account, below are some
useful links:
- [Creating password-based ServicePrincipal using CLI with ease](https://kiazhi.github.io/blog/Creating-password-based-ServicePrincipal-using-CLI-with-ease/)
- [Microsoft Docs - Create an Azure service principal with Azure CLI](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli)
- [Microsoft Docs - Create an Azure service principal with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/create-azure-service-principal-azureps)

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

## Create an Azure Storage Account for Terraform tfstate file

Although the Terraform state is generated and stored by default in a local file
named `terraform.tfstate`, but it can also be stored remotely, which works
better in a team environment where your team members share access to the state
and modify Azure Kubenetes Service (AKS) configuration.

To create an Azure Storage Account for storing the `*.tfstate` file, you can
learn on how to do that from this blog post below:
- [Creating a centralized secure storage for storing Terraform state](https://kiazhi.github.io/blog/Creating-a-centralized-secure-storage-for-Terraform-state/)

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

## Creating Terraform files to plan the Azure Kubernetes Service resources deployment

Let us start on how to create Azure Kubernetes Service (AKS) using Terraform.

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

### Create a main.tf Terraform file

Firsly, we will create a `main.tf` file to contain the following Terraform
script language to define the provider and backend configuration.

```terraform
provider "azurerm" {
  version = "~>1.5"
}

terraform {
  backend "azurerm" {}
}
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

### Create a variables.tf Terraform file

Next, you will have to create a `variables.tf` file to store configurable
variable values.

Remember that you created an AKS Service Principal account previously? Now, you
will need to replace the `default` value
"`XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`" for `client_id` and `client_secret`
variables with the previously created AKS Service Principal `AppID` and
`Password` values.

```terraform
# AKS Service Principal AppId
variable "client_id" {
  default = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}

# AKS Service Principal Password
variable "client_secret" {
  default = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}

variable "agent_count" {
  default = 3
}

variable "ssh_public_key" {
  default = "~/.ssh/id_rsa.pub"
}

variable "dns_prefix" {
  default = "k8s"
}

variable cluster_name {
  default = "cl-k8s"
}

variable resource_group_name {
  default = "rg-k8s"
}

variable location {
  default = "southeastasia"
}

variable log_analytics_workspace_name {
  default = "log-k8s"
}

# refer https://azure.microsoft.com/global-infrastructure/services/?products=monitor for log analytics available regions
variable log_analytics_workspace_location {
  default = "southeastasia"
}

# refer https://azure.microsoft.com/pricing/details/monitor/ for log analytics pricing 
variable log_analytics_workspace_sku {
  default = "PerGB2018"
}

variable tags {
  default = {
    CostCenter  = "000000"
    Department  = "IT"
    Environment = "Development"
    Importance  = "Low"
    Project     = "Kubernetes Service Blog Post"
    Owner       = "kiazhi.github.io"
  }
}
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

### Create k8s.tf Terraform file

Then, you will need to create a `k8s.tf` file where you will define your entire
AKS resources configuration incorporating with those variable values from the
`variables.tf` file.

This `k8s.tf` file demonstrates a basic AKS deployment for the need of a
project as an example. The deployment contains a resource group, log analytics
workspace, log analytics solution (ContainerInsights) and Azure Kubenetes
Service (AKS) with resource tagging that provides an end to end of the service.

```terraform
resource "azurerm_resource_group" "k8s" {
  name     = var.resource_group_name
  location = var.location
  tags     = var.tags
}

resource "random_id" "log_analytics_workspace_name_suffix" {
  byte_length = 8
}

resource "azurerm_log_analytics_workspace" "test" {
  # The WorkSpace name has to be unique across the whole of azure, not just the current subscription/tenant.
  name                = "${var.log_analytics_workspace_name}-${random_id.log_analytics_workspace_name_suffix.dec}"
  location            = var.log_analytics_workspace_location
  resource_group_name = azurerm_resource_group.k8s.name
  sku                 = var.log_analytics_workspace_sku
  tags                = var.tags
}

resource "azurerm_log_analytics_solution" "test" {
  solution_name         = "ContainerInsights"
  location              = azurerm_log_analytics_workspace.test.location
  resource_group_name   = azurerm_resource_group.k8s.name
  workspace_resource_id = azurerm_log_analytics_workspace.test.id
  workspace_name        = azurerm_log_analytics_workspace.test.name

  plan {
    publisher = "Microsoft"
    product   = "OMSGallery/ContainerInsights"
  }
}

resource "azurerm_kubernetes_cluster" "k8s" {
  name                = var.cluster_name
  location            = azurerm_resource_group.k8s.location
  resource_group_name = azurerm_resource_group.k8s.name
  dns_prefix          = var.dns_prefix
  tags                = var.tags

  linux_profile {
    admin_username  = "ubuntu"

    ssh_key {
      key_data = file(var.ssh_public_key)
    }
  }

  default_node_pool {
    name            = "agentpool"
    node_count      = var.agent_count
    vm_size         = "Standard_DS1_v2"
  }

  service_principal {
    client_id     = var.client_id
    client_secret = var.client_secret
  }

  addon_profile {
    oms_agent {
      enabled                    = true
      log_analytics_workspace_id = azurerm_log_analytics_workspace.test.id
    }
  }
}
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

### Create an output.tf Terraform file

Finally, you can create a `output.tf` file comprises of all the Terraform
provisioning outputs that you are interested and get them threw out on the
terminal at the end of the deployment.

```terraform
output "client_key" {
  value = azurerm_kubernetes_cluster.k8s.kube_config.0.client_key
}

output "client_certificate" {
  value = azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate
}

output "cluster_ca_certificate" {
  value = azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate
}

output "cluster_username" {
  value = azurerm_kubernetes_cluster.k8s.kube_config.0.username
}

output "cluster_password" {
  value = azurerm_kubernetes_cluster.k8s.kube_config.0.password
}

output "kube_config" {
  value = azurerm_kubernetes_cluster.k8s.kube_config_raw
}

output "host" {
  value = azurerm_kubernetes_cluster.k8s.kube_config.0.host
}
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

## Deploying Azure Kubernetes Service (AKS) to Azure using Terraform

Once those Terraform (`*.tf`) files are created, you will need to initialize
the working directory containing those Terraform configuration files using the
`terraform init` command.

```sh
terraform init \
  -backend-config="storage_account_name=$storage_account_name" \
  -backend-config="container_name=$storage_account_container_name" \
  -backend-config="access_key=$storage_account_key" \
  -backend-config="key=terraform.tfstate"
```

Next, you will need to use a Service Principal account with Contributor role
which `terraform` command can utilise to communicate with Azure and initiate
the deployment. You will need store your Service Principal App ID for
`ARM_CLIENT_ID`, Password for `ARM_CLIENT_SECRET`, SubscriptionID for
`ARM_SUBSCRIPTION_ID` and Tenant Id for `ARM_TENANT_ID` environment variables
in your terminal.

```sh
export \
  ARM_CLIENT_ID="XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" \
  ARM_CLIENT_SECRET="XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" \
  ARM_SUBSCRIPTION_ID="XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" \
  ARM_TENANT_ID="XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" ;
```

Then, you will use the `terrform plan` command to check the execution plan and
generate an output of the generated plan.

```sh
terraform plan -out out.plan
```

Finally, you are use `terraform apply` command to apply the generated plan
changes to Azure.

```sh
terraform apply out.plan
```

Once the command execution completed, you will see the resource group
containing the Azure Kurbernetes Service (AKS) and other resources deployed in
the Azure Portal.

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

- [Microsoft Docs - Create a Kubernetes cluster with Azure Kubernetes Service using Terraform](https://docs.microsoft.com/en-us/azure/terraform/terraform-create-k8s-cluster-with-tf-and-aks)
- [Terraform Docs - Configuration Language](https://www.terraform.io/docs/configuration/index.html)
- [Terraform CLI - terraform init](https://www.terraform.io/docs/commands/init.html)
- [Terraform CLI - terraform plan](https://www.terraform.io/docs/commands/plan.html)
- [Terraform CLI - terraform apply](https://www.terraform.io/docs/commands/apply.html)
- [Getting started with Azure deployment using Terraform](https://kiazhi.github.io/blog/Getting-started-with-Azure-deployment-using-Terraform/)
- [Creating a centralized secure storage for storing Terraform state](https://kiazhi.github.io/blog/Creating-a-centralized-secure-storage-for-Terraform-state/)

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
