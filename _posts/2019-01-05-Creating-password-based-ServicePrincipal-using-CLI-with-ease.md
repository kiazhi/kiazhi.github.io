---
layout: single
title: "Creating password-based ServicePrincipal using CLI with ease"
excerpt: "Sharing my way of creating a password-based ServicePrincipal in Azure
using CLI to quickly carry on scripting and testing in the current terminal
session easily."
author: Ryen Tang
date: 2019-01-05 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Azure
  - AzCLI
  - PowerShell
  - "Ryen Tang"
---

Sharing my way of creating a password-based ServicePrincipal in Azure using CLI
to quickly carry on scripting and testing in the current terminal session
without losing the identity and password.

Of course, you can save the password generated and save it some where safe to
continue using ServicePrincipal account to perform tasks for automation.

It is a snippet of codes to make life easy.

## Creating an Azure ServicePrincipal

This is how to create a password-based ServicePrincipal account in Azure. You
can choose to use `AzCLI` or `PowerShell` with `Az` PowerShell module.

### AzCLI

```sh

echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
  "Create password-based ServicePrincipal" ;

echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
  "Log in as Azure global administrator" ;

az login ;

export \
  service_principal_display_name='sp-cli' \
\
  password=$(az ad sp create-for-rbac \
  --name "$service_principal_display_name" \
  --query 'password' \
  --output 'tsv') \
\
 app_id=$(az ad sp show \
  --id "http://$service_principal_display_name" \
  --query 'appId' \
  --output 'tsv') \
\
 tenant_id=$(az ad sp show \
  --id "http://$service_principal_display_name" \
  --query 'appOwnerTenantId' \
  --output 'tsv') ;

echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
  "Log out Azure global administrator" ;

az logout ;

if [ "$(az login \
  --service-principal \
  --username $app_id \
  --password $password \
  --tenant $tenant_id \
  --query '[0].tenantId' \
  --output 'tsv')" = $tenant_id ] ;
then \
  echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
    "ServicePrincipal password-based creation succeeded." ;

  echo "# Please remember to copy the password and keep it safe." ;

  echo "AppId: $app_id" ;
  
  echo "DisplayName: $service_principal_display_name" ;
  
  export \
    service_principal_name="http://$service_principal_display_name" ;

  echo "Name: $service_principal_name" ;
  
  echo "Password: $password" ;
  
  echo "TenantId: $tenant_id" ;
else \

  echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
    "ServicePrincipal password-based creation failed." ;

fi ;

echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
    "Display current session login user detail." ;

az account show \
  --query user \
  --output tsv ;

# Do something using ServicePrincipal account privileges

```

### PowerShell

```powershell

Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Create password-based ServicePrincipal") ;

Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Log in as Azure global administrator") ;

Connect-AzAccount ;

$ServicePrincipalDisplayName = "sp-cli" ;

$TenantId = (Get-AzContext).Tenant.Id ;

$ServicePrincipal = New-AzADServicePrincipal `
  -DisplayName $ServicePrincipalDisplayName ;

$Password = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto( `
  [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR( `
    $ServicePrincipal.Secret)) ;

Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Log out Azure global administrator") ;

Logout-AzAccount ;

if($(Connect-AzAccount `
  -ServicePrincipal `
  -Credential $(New-Object `
    -TypeName "System.Management.Automation.PSCredential" `
    -ArgumentList ("$($ServicePrincipal.ApplicationId)", `
        (ConvertTo-SecureString `
            -String "$($Password)" `
            -AsPlainText `
            -Force))) `
  -Tenant "$($TenantId)" `
  -Force).Context.Account.Id -eq $ServicePrincipal.ApplicationId) `
{ `
  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "ServicePrincipal password-based creation succeeded.") ;

  Write-Host "# Please remember to copy the password and keep it safe." ;

  Write-Host "AppId: $($ServicePrincipal.ApplicationId)" ;

  Write-Host "DisplayName: $($ServicePrincipalDisplayName)" ;

  Write-Host "Name: $($ServicePrincipal.ServicePrincipalNames[1])" ;

  Write-Host "Password: $($Password)" ;

  Write-Host "TenantId: $($TenantId)" ;

} `
else `
{ `
  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "ServicePrincipal password-based creation failed.") ;
} ;

Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Display current session login user detail.") ;

$(Get-AzContext).Account `
  | Select-Object `
    -Property `
      Id, `
      Type ; 

# Do something using ServicePrincipal account privileges

```

## Removing Azure ServicePrincipal

This is how to remove the password-based ServicePrincipal account in Azure. You
can choose to use `AzCLI` or `PowerShell` with `Az` PowerShell module.

### AzCLI

```sh

# After doing something using ServicePrincipal account privileges

echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
  "Remove password-based ServicePrincipal" ;

echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
  "Log in as Azure global administrator" ;

az login ;

if [ "$(az ad sp show \
  --id "http://$service_principal_display_name" \
  --query 'appId' \
  --output 'tsv')" = $app_id ] ;
then \

  echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
    "ServicePrincipal Application Id ($app_id) found." ;

  echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
    "Deleting ServicePrincipal" ;

  az ad sp delete \
    --id $app_id ;

else \

  echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
    "ServicePrincipal Application Id ($app_id) not found." ;

fi ;

echo "# $(date '+%Y-%m-%d %H:%M:%S %z') -" \
  "Log out Azure global administrator" ;

az logout ;

```

### PowerShell

```powershell

# After doing something using ServicePrincipal account privileges

Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Remove password-based ServicePrincipal") ;

Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Log in as Azure global administrator") ;

Connect-AzAccount ;

if($(Get-AzADServicePrincipal `
  -DisplayName $ServicePrincipalDisplayName).ApplicationId `
    -eq `
  $ServicePrincipal.ApplicationId) `
{ `
  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "ServicePrincipal Application Id ($($ServicePrincipal.ApplicationId)) found.") ;
  
  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
      "Deleting ServicePrincipal") ;

  Remove-AzADServicePrincipal `
    -ApplicationId $ServicePrincipal.ApplicationId `
    -Force ;

} `
else `
{ `
  Write-Host $("{0}{1}" `
    -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "ServicePrincipal Application Id ($($ServicePrincipal.ApplicationId)) not found.") ;
} ;

Write-Host $("{0}{1}" `
  -f "# $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss zzz') - ", `
    "Log out Azure global administrator") ;

Logout-AzAccount `
  | Out-Null ;

```

