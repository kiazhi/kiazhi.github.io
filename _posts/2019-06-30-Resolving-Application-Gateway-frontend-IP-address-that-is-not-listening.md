---
layout: single
title: "Resolving Application Gateway frontend listener that is not listening"
excerpt: "Have you encounter Azure Application Gateway listener stopped
listening? How do you resolve such issue if no change in configuration had been
made recently?"
author: Ryen Tang
date: 2019-06-30 00:00:00 +1200
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
  - Troubleshooting
---

This scenario is what I actually encountered and took me a few hours to
understand, troubleshoot and resolving it. There was an Azure Application
Gateway that did not had any configuration changes for the past few months.
Although there were some Network Security Groups (NSGs) rule changes to the
Virtual Network (VNet) subnet where the Application Gateway is connected, it
might have caused the issue and the frontend listener stopped listening.

## Scenario

What is the condition?

- The Application Gateway has always been working fine without issue until now
- No configuration change was applied to the Application Gateway recently
- Application Gateway Health Status shows Healthy state

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

What is the current issue?

- Discovery of Application Gateway Frontend Listener is not listening or
responding

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

## What is needed to resolve this with minimal impact or change?

This is how I try resolving the Application Gateway Frontend Listener issue.

### Find out Application Gateway Frontend IP Configuration

Firstly when someone escalate to you a problem with their website, I will have
to find out where it is hosted and what is the IP address that is related to
the URL.

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

#### AzCLI

In this section, I will demonstrate an example on how to find out the
Application Gateway's frontend IP address using AzCLI.

```sh
az network application-gateway frontend-ip list \
  -g '$RESOURCE_GROUP_NAME' \
  --gateway-name '$APPLICATION_GATEWAY_NAME'
```

Once the command is executed, it will return the following similar output below
where you can identify the IP address allocated to the Application Gateway's
frontend IP configuration.

```text
{
  "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/rg-application-gateway/providers/Microsoft.Network/applicationGateways/appgateway-webapp/frontendIPConfigurations/appgateway-frontend-private-ip",
  "name": "appgateway-frontend-private-ip",
  "privateIpAddress": "10.0.0.10",
  "privateIpAllocationMethod": "Static",
  "provisioningState": "Succeeded",
  "publicIpAddress": null,
  "resourceGroup": "rg-application-gateway",
  "subnet": {
    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/rg-vnet-intranet/providers/Microsoft.Network/virtualNetworks/vnet-intranet/subnets/sub-intranet-loadbalancer",
    "resourceGroup": "rg-vnet-intranet"
  },
  "type": "Microsoft.Network/applicationGateways/frontendIPConfigurations"
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

#### PowerShell

In this section, I will demonstrate an example on how to find out the
Application Gateway's frontend IP address using PowerShell Az module.

```powershell
Get-AzApplicationGatewayFrontendIPConfig `
  -ApplicationGateway (Get-AzApplicationGateway `
    -Name "$APPLICATION_GATEWAY_NAME" `
    -ResourceGroupName "$RESOURCE_GROUP_NAME") ;
```

Once the command is executed, it will return the following similar output below
where you can identify the IP address allocated to the Application Gateway's
frontend IP configuration.

```text
PrivateIPAddress          : 10.0.0.10
PrivateIPAllocationMethod : Static
Subnet                    : Microsoft.Azure.Commands.Network.Models.PSResourceId
PublicIPAddress           : 
ProvisioningState         : Succeeded
Type                      : Microsoft.Network/applicationGateways/frontendIPConfigurations
SubnetText                : {
                              "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/rg-vnet-intranet/providers/Microsoft.Network/virtualNetworks/vnet-intranet/subnets/sub-intranet-loadbalancer"
                            }
PublicIpAddressText       : null
Name                      : appgateway-frontend-private-ip
Etag                      : W/"9da8e6dc-d4d5-44b9-be52-20c657e8d4bc"
Id                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/rg-application-gateway/providers/Microsoft.Network/applicationGateways/appgateway-webapp/frontendIPConfigurations/appgateway-frontend-private-ip
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

### Test the IP Address is listening or responding

You can try to use a web browser to launch the website using the IP Address
<`http://10.0.0.10/`> or try using `curl` command from a Linux machine from
`Bash`.

```sh
curl 10.0.0.10
```

In the example if the Application Gateway's frontend IP address listener is not
working, you will encounter the following similar output from the `curl`
command below.

```text
curl: (7) Failed to connect to 10.0.0.10 port 80: Connection timed out
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

### Resolving the issue without making any configuration change

Since Application Gateway is offered as an Azure PaaS type of service, there
isn't a reboot/restart button on the portal. Although technically we know that
it must be system instance (or VM) behind the scene, it is an offering that
provides scalability and my Application Gateway has 2 instances.

Since there are 2 instances, the frontend IP address is a virtual IP (VIP),
therefore the 2 instances will have their own IP addresses too. I probe the
subnet remaining IP addresses for the 2 instance's IP addresses and found out
that it is using 10.0.0.2 and 10.0.0.3 as an example. When I initiated the
`curl` command on 10.0.0.2 and 10.0.0.3, there is a response back with the
website content.

But I still could not get the frontend IP address to work and I know that a
restart of the Application Gateway is required.

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

#### AzCLI

In this section, I will demonstrate an example on how to restart an Application
Gateway using AzCLI. Firstly, you will need to manually stop the Application
Gateway with the command below.

```sh
az network application-gateway stop \
  -g 'gt-gccs-azuredashboard-iz-app' \
  -n 'appgateway-gcc-azure-dashboard'
```

Once the Application Gateway has stopped, you will need to manually start the
Application Gateway with the command below.

```sh
az network application-gateway start \
  -g 'gt-gccs-azuredashboard-iz-app' \
  -n 'appgateway-gcc-azure-dashboard'
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

#### PowerShell

In this section, I will demonstrate an example on how to restart an Application
Gateway using Az PowerShell module. Firstly, you will need to manually stop the
Application Gateway with the command below.

```powershell
Stop-AzApplicationGateway `
  -ApplicationGateway (Get-AzApplicationGateway `
    -Name "$APPLICATION_GATEWAY_NAME" `
    -ResourceGroupName "$RESOURCE_GROUP_NAME") ;
```

Once the Application Gateway has stopped, you will need to manually start the
Application Gateway with the command below.

```powershell
Start-AzApplicationGateway `
  -ApplicationGateway (Get-AzApplicationGateway `
    -Name "$APPLICATION_GATEWAY_NAME" `
    -ResourceGroupName "$RESOURCE_GROUP_NAME") ;
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

### Resolving the issue with minor configuration change

Although this method is not the best recommended solution because configuration
changes must be made.

But if you don't really have the luxury to use the AzCLI or Az PowerShell
module except through the Azure Portal, this is the only method to restart the
Application Gateway with minimal changes without making any major functionality
configuration change that may impact the loadbalancing functionality.

To do this on the Azure Portal:
1. Launch Azure Portal
2. Locate your Application Gateway resource
3. Navigate to Settings -> Configuration
4. Modify the Instance Count (Eg. Change from 2 to 1)
5. Select Save

This will cause a restart of Application Gateway. Once the Application Gateway
completed updating configuration, you will need to repeat the steps 4 and 5
above to modify the Instance Count back to the original value (Eg. Change from
1 to 2).

Until Microsoft decided to add a Restart button on the Azure Portal for
Application Gateway, this might be the only way to restart it using the portal.

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

- [Microsoft Docs - az network application-gateway frontend-ip list](https://docs.microsoft.com/en-us/cli/azure/network/application-gateway/frontend-ip?view=azure-cli-latest#az-network-application-gateway-frontend-ip-list)
- [Microsoft Docs - Get-AzApplicationGatewayFrontendIPConfig](https://docs.microsoft.com/en-us/powershell/module/az.network/get-azapplicationgatewayfrontendipconfig)
- [Microsoft Docs - az network application-gateway start](https://docs.microsoft.com/en-us/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-start)
- [Microsoft Docs - az network application-gateway stop ](https://docs.microsoft.com/en-us/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-stop)
- [Microsoft Docs - Start-AzApplicationGateway](https://docs.microsoft.com/en-us/powershell/module/az.network/start-azapplicationgateway)
- [Microsoft Docs - Stop-AzApplicationGateway](https://docs.microsoft.com/en-us/powershell/module/az.network/stop-azapplicationgateway)

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
