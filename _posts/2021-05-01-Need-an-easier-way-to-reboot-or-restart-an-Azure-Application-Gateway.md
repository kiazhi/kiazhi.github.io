---
layout: single
title: "Need an easier way to reboot or restart an Azure Application Gateway?"
excerpt: "When your Azure Application Gateway Platform-as-a-Service (PaaS) has
an issue and you need to reboot the AzApplicationGateway. I made a cmdlet instead."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2021-05-01 00:00:00 +0800
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

In this scenario, I have an Azure Application Gateway (AzApplicationGateway)
Platform-as-a-Service (PaaS) resource. I have experience an issue where I did
not observe any incoming traffic on my logs after some minor changes to the
load balancer rules and I want to restart or reboot my load balancer. But there
isn't any restart or reboot options in Azure portal, remembered I had
previously published a blog post
[here](https://kiazhi.github.io/blog/Resolving-Application-Gateway-frontend-IP-address-that-is-not-listening/)
and I have just developed a `Restart-AzApplicationGateway` PowerShell cmdlet
for it.

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

I need an easier way to restart or reboot an Azure Application Gateway
(AzApplicationGateway).

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

1. Create a Restart-AzApplicationGateway.psm1 file

2. Copy and paste the code below into the file and save it

```powershell
<#
 .Synopsis
  Restart an Azure Application Gateway.

 .Description
  Restart an Azure Application Gateway. This function retains the existing
  Azure Application Gateway configurations prior to stop the Azure Application
  Gateway and start the Azure Application Gateway.

 .Parameter Name
  Specifies the name of the Azure Application Gateway that this cmdlet will restarts.

 .Parameter ResourceGroupName
  Specifies the name of a resource group containing the Azure Application Gateway.

 .Parameter Wait
  Specifies the number of seconds to wait before starting the Azure Application Gateway.

 .Example
  # Restart the Azure Application Gateway.
  Restart-AzApplicationGateway -Name "myAzureApplicationGateway" -ResourceGroupName "myResourceGroup"

 .Example
  # Restart the Azure Application Gateway with Verbose outputs.
  Restart-AzApplicationGateway -Name "myAzureApplicationGateway" -ResourceGroupName "myResourceGroup" -Verbose

 .Example
  # Restart the Azure Application Gateway and wait for 30 seconds duration with Verbose outputs.
  Restart-AzApplicationGateway -Name "myAzureApplicationGateway" -ResourceGroupName "myResourceGroup" -Wait 30 -Verbose

 .INPUTS
 	System.String

 .OUTPUTS
 	System.Object

 .NOTES
  Author: Ryen Tang
	GitHub: https://github.com/kiazhi
  
#>

function Restart-AzApplicationGateway {

	[CmdletBinding()]

	param (
		[Parameter( 
      Mandatory=$True, 
      ValueFromPipeline=$True, 
      ValueFromPipelineByPropertyName=$True)] 
		[String] $Name,

		[Parameter( 
      Mandatory=$True, 
      ValueFromPipeline=$True, 
      ValueFromPipelineByPropertyName=$True)] 
		[String] $ResourceGroupName,

		[Parameter( 
      Mandatory=$False, 
      ValueFromPipeline=$True, 
      ValueFromPipelineByPropertyName=$True)] 
		[Int] $Wait
	)

	begin {}

	process {

		$AzApplicationGateway = Get-AzApplicationGateway `
			-Name $Name `
			-ResourceGroupName $ResourceGroupName

		if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
			Write-Verbose `
				-Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
					+ "Get existing AzApplicationGateway configurations:")

			Write-Output `
				-InputObject $AzApplicationGateway
		}

		if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
			Write-Verbose -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
				+ "Stopping AzApplicationGateway")
		}

		Stop-AzApplicationGateway `
			-ApplicationGateway $AzApplicationGateway `
		| Select-Object `
			-Property `
				Name, `
				ResourceGroupName, `
				OperationalState, `
				ProvisioningState

		if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
			Write-Verbose -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
				+ "Stopped AzApplicationGateway")
		}

		if($Wait) {

			if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
				Write-Verbose -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
					+ "Start waiting for $Wait seconds")
			}

			Start-Sleep -Seconds $Wait

			if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
				Write-Verbose -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
					+ "End waiting")
			}
		}

		if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
			Write-Verbose -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
				+ "Starting AzApplicationGateway")
		}

		Start-AzApplicationGateway `
			-ApplicationGateway $AzApplicationGateway `
		| Select-Object `
			-Property `
				Name, `
				ResourceGroupName, `
				OperationalState, `
				ProvisioningState

		if($PSCmdlet.MyInvocation.BoundParameters["Verbose"].IsPresent) {
			Write-Verbose -Message $("$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzzz') - " `
        + "Started AzApplicationGateway")
		}

  }

	end {}

}
```

3. Import the Restart-AzApplicationGateway.psm1 module

4. Type the following below.

```powershell
Restart-AzApplicationGateway -Name "myAzureApplicationGateway" -ResourceGroupName "myAzureApplicationGatewayResourceGroup" -Verbose
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

If you are interested with the `Restart-AzApplicationGateway.psm1` source code,
it is currently published on Github's
[kiazhi/Restart-AzApplicationGateway](https://github.com/kiazhi/Restart-AzApplicationGateway)
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

- [Microsoft Docs - Start-AzApplicationGateway](https://docs.microsoft.com/en-us/powershell/module/az.network/start-azapplicationgateway)
- [Microsoft Docs - Stop-AzApplicationGateway](https://docs.microsoft.com/en-us/powershell/module/az.network/stop-azapplicationgateway)
- [Github - kiazhi/Restart-AzApplicationGateway](https://github.com/kiazhi/Restart-AzApplicationGateway)

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
