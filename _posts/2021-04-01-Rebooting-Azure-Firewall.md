---
layout: single
title: "Rebooting Azure Firewall"
excerpt: "When your Azure Firewall Platform-as-a-Service (PaaS) has an issue
and you wish to reboot the AzFirewall. This is how to I did it with PowerShell."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2021-04-01 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - Cloud
  - Azure
  - PowerShell
  - "Ryen Tang"
  - Troubleshooting
---

In this scenario, I have an Azure Firewall (AzFirewall) Platform-as-a-Service
(PaaS) resource. I have experience an issue where I cannot observe any incoming
traffic on my logs after some minor changes to the firewall rules and I want to
restart or reboot my firewall. But there isn't any restart or reboot options in
Azure, that's where I explored and found that Azure PowerShell can allocate and
deallocate the resource, and created this `Restart-AzFirewall` cmdlet instead.

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

## Problem

I am unable to restart or reboot an Azure Firewall (AzFirewall).

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

## Solution

1. Create a Restart-AzFirewall.psm1 file

2. Copy and paste the code below into the file and save it

```powershell
<#
.Synopsis
  Restarts an Azure Firewall.
.Description
  Restarts an Azure Firewall. This function retains the existing Azure Firewall
  configurations prior to deallocating the resource to stop the Azure Firewall
  and reallocating the resource with those previously retained Azure Firewall
  configurations to start the Azure Firewall.
.Parameter Name
  Specifies the name of the Azure Firewall that this cmdlet will restarts.
.Parameter ResourceGroupName
  Specifies the name of a resource group to contain the Firewall.
.Example
  # Restart the Azure Firewall.
  Restart-AzFirewall -Name "myAzureFirewall" -ResourceGroupName "myResourceGroup"
.Example
  # Restart the Azure Firewall with Verbose outputs.
  Restart-AzFirewall -Name "myAzureFirewall" -ResourceGroupName "myResourceGroup" -Verbose
.INPUTS
  System.String
.OUTPUTS
  System.Object
.NOTES
  Author: Ryen Tang
  GitHub: https://github.com/kiazhi
  
#>

function Restart-AzFirewall {

  [CmdletBinding()]

  param (
    [Parameter(Mandatory)]
    [String] $Name,

    [Parameter(Mandatory)]
    [String] $ResourceGroupName
  )

  begin {}

  process {

    $AzFirewall = Get-AzFirewall `
      -Name $Name `
      -ResourceGroupName $ResourceGroupName

    $ExistingPublicIpAddressName = (Get-AzResource -ResourceId (((Get-AzFirewall `
      -Name $Name `
      -ResourceGroupName $ResourceGroupName).IpConfigurations).PublicIpAddress).Id).Name

    if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
        Write-Verbose `
          -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
            + "Existing azFirewall Public Ip Address Name: $ExistingPublicIpAddressName")
    }

    $ExistingPublicIpAddressResourceGroupName = (Get-AzResource -ResourceId (((Get-AzFirewall `
      -Name $Name `
      -ResourceGroupName $ResourceGroupName).IpConfigurations).PublicIpAddress).Id).ResourceGroupName

    if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
        Write-Verbose `
          -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
            + "Existing azFirewall Public Ip Address Resource Group Name: $ExistingPublicIpAddressResourceGroupName")
    }

    $ExistingVirtualNetworkName = (Get-AzResource `
      -Name $(((Get-AzResource -ResourceId (((Get-AzFirewall `
        -Name $Name `
        -ResourceGroupName $ResourceGroupName).IpConfigurations).Subnet).Id)).ParentResource -replace '.*/','') `
      -ResourceType 'Microsoft.Network/virtualNetworks').Name

    if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
        Write-Verbose `
          -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
            + "Existing azFirewall Virtual Network Name: $ExistingVirtualNetworkName")
    }

    $ExistingVirtualNetworkResourceGroupName = (Get-AzResource `
      -Name $(((Get-AzResource -ResourceId (((Get-AzFirewall `
        -Name $Name `
        -ResourceGroupName $ResourceGroupName).IpConfigurations).Subnet).Id)).ParentResource -replace '.*/','') `
      -ResourceType 'Microsoft.Network/virtualNetworks').ResourceGroupName

    if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
        Write-Verbose `
          -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
            + "Existing azFirewall Virtual Network Resource Group Name: $ExistingVirtualNetworkResourceGroupName")
    }

    $AzFirewall.Deallocate()

    if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
        Write-Verbose -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
          + "Stopping azFirewall")
    }

    Set-AzFirewall `
      -AzureFirewall $AzFirewall

    if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
        Write-Verbose -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
          + "Stopped azFirewall")
    }

    $VirtualNetwork = Get-AzVirtualNetwork `
      -Name $ExistingVirtualNetworkName `
      -ResourceGroupName $ExistingVirtualNetworkResourceGroupName

    $PublicIpAddress = Get-AzPublicIpAddress `
      -Name $ExistingPublicIpAddressName `
      -ResourceGroupName $ExistingPublicIpAddressResourceGroupName

    $AzFirewall.Allocate($VirtualNetwork,$PublicIpAddress)

    if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
        Write-Verbose -Message "$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - Starting azFirewall"
    }

    Set-AzFirewall -AzureFirewall $AzFirewall

    if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
        Write-Verbose -Message "$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - Started azFirewall"
    }

  }

  end {}
  
}
```

3. Import the Restart-AzFirewall.psm1 module

4. Type the following below.

```powershell
Restart-AzFirewall -Name "myAzureFirewall" -ResourceGroup "myAzureFirewallResourceGroup" -Verbose
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

## Update

If you are interested with the `Restart-AzFirewall.psm1` source code, it is
currently published on Github's [kiazhi/Restart-AzFirewall](https://github.com/kiazhi/Restart-AzFirewall)
repository. Hope it helps to make your life easier.

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

- [Microsoft Docs - Set-AzFirewall - Deallocate and Allocate](https://docs.microsoft.com/en-us/powershell/module/az.network/set-azfirewall?view=azps-7.2.0#4--deallocate-and-allocate-the-firewall)
- [Github - kiazhi/Restart-AzFirewall](https://github.com/kiazhi/Restart-AzFirewall)

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
