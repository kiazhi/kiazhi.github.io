---
layout: single
title: "Communicating using TLS 1.2 or TLS 1.1 securely with PowerShell"
excerpt: "Having issue establishing secure communication? Are you still
configured with SSL 3.0 and TLS 1.0? This is how to configure TLS 1.2 on
PowerShell."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-07-23 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - PowerShell
  - "Ryen Tang"
---

Ever tried downloading PowerShell Core directly from GitHub and receiving
`The request was aborted: Could not create SSL/TLS secure channel.` error
message responded back?

**Examples:**

```powershell
# An example using Invoke-WebRequest to download package from GitHub
Invoke-WebRequest `
    -Uri 'https://github.com/PowerShell/PowerShell/releases/download/v6.0.3/PowerShell-6.0.3-win-x64.msi' `
    -OutFile 'C:\Temp\PowerShell-6.0.3-win-x64.msi' ;
```

```powershell
# An example using Invoke-RestMethod against GitHub API
Invoke-RestMethod `
    -Uri https://api.github.com/ `
    -Method Get ;
```

**Output:**

```text
Invoke-WebRequest : The request was aborted: Could not create SSL/TLS secure channel.
At line:1 char:1
+ Invoke-WebRequest `
+ ~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand
```

Since the technology has moved on with SSL 3.0 and TLS 1.0 being deprecated,
you may be woke up with an alarming realization that your code needs updating
to support the use of TLS 1.2 (Recommended) or TLS 1.1 (Minimum) in order to
get them (Eg. `Invoke-WebRequest`, `Invoke-RestMethod`) to work. While some
service providers (eg.
[GitHub](https://githubengineering.com/crypto-removal-notice/)) drops
SSL 3.0, TLS 1.0 and TLS 1.1 in favour of only TLS 1.2 or higher.

## What actually went wrong?

There is nothing particularly wrong except you might not be aware of your
current Transport Layer Security protocols configuration on the environment.

In order to find out the current Transport Layer Security protocols
configuration on your environment, you can obtain a list of protocols
configured on your environment by using the command in PowerShell below.

```powershell
# List the Security Protocol configuration
[Net.ServicePointManager]::SecurityProtocol
```

**Output:**

```text
Ssl3, Tls
```

If your output does not have `Tls12` or `Tls11` representing TLS 1.2 or TLS 1.1
listed, it means that your PowerShell console will not communicate or negotiate
in those Transport Layer Security protocols with the remote resource.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## How to enable TLS 1.2 and TLS 1.1 protocols

To overcome the communication issue with GitHub, you will need to enable TLS 1.2
protocol.

```powershell
# Enable TLS 1.2 as Security Protocol
[Net.ServicePointManager]::SecurityProtocol = `
    [Net.SecurityProtocolType]::Tls12 ;
```

But if you find that you can include all Transport Layer Security protocols
that are not deprecated yet, you can input them by separating each protocol
with a comma (`,`) like below.

```powershell
# Enable TLS 1.2 and TLS 1.1 as Security Protocols
[Net.ServicePointManager]::SecurityProtocol = `
    [Net.SecurityProtocolType]::Tls12,
    [Net.SecurityProtocolType]::Tls11 ;
```

Once you have configured the required Transport Layer Security protocol for the
communication, you can try listing them to validate they have been configured
and test the communication again to determine if the secure communication can
be established.

```powershell
# Validate the configured protocol(s) is/are listed
[Net.ServicePointManager]::SecurityProtocol

# Try establishing secure communication again
Invoke-RestMethod `
    -Uri https://api.github.com/ `
    -Method Get ;
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Understanding Transport Layer Security Protocols

Transport Layer Security protocols are actually crytographic protocols that
provide communication security over the network. The primary aim is to provide
privacy and data integrity between two or more machines or applications where
the connection is private because of symmetric cryptography used to encrypt the
data transmitted.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Deprecated Transport Layer Security Protocols

Saying goodbye to SSL 2.0, 3.0 and TLS 1.0 Transport Layer Security protocols
as they are considered as deprecated protocols because of vulnerabilities.

- [SSL 2.0](https://tools.ietf.org/html/rfc6176) introduced since February 1995
and deprecated on March 2011
- [SSL 3.0](https://tools.ietf.org/html/rfc7568) introduced since March 1996
and deprecated on June 2015
- [TLS 1.0](https://tools.ietf.org/html/rfc2246) introduced since January 1999
and deprecated on June 2018

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Currently supported Transport Layer Security Protocols

These are still supported protocols that does not have any known protocol
vulnerabilities during the time this blog post is published.

- [TLS 1.1](https://tools.ietf.org/html/rfc4346) introduced since April 2006
- [TLS 1.2](https://tools.ietf.org/html/rfc5246) introduced since August 2008

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Next version of Transport Layer Security Protocol

During April 2014, a draft for TLS 1.3 specification was introduced and has
been revised numerous times till today.

- [TLS 1.3](https://tools.ietf.org/html/draft-ietf-tls-tls13-00) introduced
since April 2014

Since then at the time of this blog post, the current draft version of TLS 1.3
is at [28](https://tools.ietf.org/html/draft-ietf-tls-tls13-28) superseding
draft version
[27](https://tools.ietf.org/html/draft-ietf-tls-tls13-27), 
[26](https://tools.ietf.org/html/draft-ietf-tls-tls13-26), 
[25](https://tools.ietf.org/html/draft-ietf-tls-tls13-25), 
[24](https://tools.ietf.org/html/draft-ietf-tls-tls13-24), 
[23](https://tools.ietf.org/html/draft-ietf-tls-tls13-23), 
[22](https://tools.ietf.org/html/draft-ietf-tls-tls13-22), 
[21](https://tools.ietf.org/html/draft-ietf-tls-tls13-21), 
[20](https://tools.ietf.org/html/draft-ietf-tls-tls13-20), 
[19](https://tools.ietf.org/html/draft-ietf-tls-tls13-19), 
[18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18), 
[17](https://tools.ietf.org/html/draft-ietf-tls-tls13-17), 
[16](https://tools.ietf.org/html/draft-ietf-tls-tls13-16), 
[15](https://tools.ietf.org/html/draft-ietf-tls-tls13-15), 
[14](https://tools.ietf.org/html/draft-ietf-tls-tls13-14), 
[13](https://tools.ietf.org/html/draft-ietf-tls-tls13-13), 
[12](https://tools.ietf.org/html/draft-ietf-tls-tls13-12), 
[11](https://tools.ietf.org/html/draft-ietf-tls-tls13-11), 
[10](https://tools.ietf.org/html/draft-ietf-tls-tls13-10), 
[09](https://tools.ietf.org/html/draft-ietf-tls-tls13-09), 
[08](https://tools.ietf.org/html/draft-ietf-tls-tls13-08), 
[07](https://tools.ietf.org/html/draft-ietf-tls-tls13-07), 
[06](https://tools.ietf.org/html/draft-ietf-tls-tls13-06), 
[05](https://tools.ietf.org/html/draft-ietf-tls-tls13-05), 
[04](https://tools.ietf.org/html/draft-ietf-tls-tls13-04), 
[03](https://tools.ietf.org/html/draft-ietf-tls-tls13-03), 
[02](https://tools.ietf.org/html/draft-ietf-tls-tls13-02), 
[01](https://tools.ietf.org/html/draft-ietf-tls-tls13-01) and 
[00](https://tools.ietf.org/html/draft-ietf-tls-tls13-00).

If you are interested in participating in the TLS 1.3 specification, you can
participate on the following GitHub
[TLSWG/TLS13-Spec](https://github.com/tlswg/tls13-spec) repository.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Configuring Transport Layer Security protocols for PowerShell console

Since Transport Layer Security protocols on PowerShell may be configured with
just `ssl` (SSL 3.0) and `tls` (TLS 1.0) by default, you may not want to
constantly manually configure the `[Net.ServicePointManager]::SecurityProtocol`
every time you launches your PowerShell console.

This section will demonstrates on how to configure PowerShell console with
Transport Layer Security protocols persistently.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### TLS 1.1 and TLS 1.2

To apply persistent configuration of TLS 1.1 and TLS 1.2 to your PowerShell
console, you will have to define `[Net.ServicePointManager]::SecurityProtocol`
with the appropriate `[Net.SecurityProtocolType]::Tls11` (TLS 1.1) and
`[Net.SecurityProtocolType]::Tls12` (TLS 1.2) properties to a
`Microsoft.PowerShell_Profile.ps1` file.

> Note: It is highly recommended to exclude SSL 3.0 and TLS 1.0 usage if your
environment is up to date and only uses TLS 1.1 or TLS 1.2 Transport Layer
security protocols.

```powershell
@"
# Configure PowerShell Transport Layer Security Protocols
[Net.ServicePointManager]::SecurityProtocol = `
    [Net.SecurityProtocolType]::Tls11,
    [Net.SecurityProtocolType]::Tls12 ;
"@ | `
Out-File `
    -FilePath "$ENV:USERPROFILE\Documents\WindowsPowerShell\Microsoft.PowerShell_Profile.ps1" `
    -Append
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### SSL 3.0, TLS 1.0, TLS 1.1 and TLS 1.2

If you have to work with legacy Transport Layer Security protocols because your
internal environment have not been upgraded to compliant with the new standard
yet, you can still configure your PowerShell console to include those legacy
SSL 3.0 and TLS 1.0 Transport Layer Security protocols.

> Note: It is still highly recommended that you upgrade your environment to
support TLS 1.2 or TLS 1.1 and stop using SSL 3.0 and TLS 1.0.

```powershell
@"
# Configure PowerShell Transport Layer Security Protocols
[Net.ServicePointManager]::SecurityProtocol = `
    [Net.SecurityProtocolType]::Ssl3,
    [Net.SecurityProtocolType]::Tls,
    [Net.SecurityProtocolType]::Tls11,
    [Net.SecurityProtocolType]::Tls12 ;
"@ | `
Out-File `
    -FilePath "$ENV:USERPROFILE\Documents\WindowsPowerShell\Microsoft.PowerShell_Profile.ps1" `
    -Append
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Configuring Transport Layer Security protocols for PowerShell ISE

Since Transport Layer Security protocols on PowerShell may be configured with
just `ssl` (SSL 3.0) and `tls` (TLS 1.0) by default, you may not want to
constantly manually configure the `[Net.ServicePointManager]::SecurityProtocol`
every time you launches your PowerShell ISE.

This section will demonstrates on how to configure PowerShell ISE with
Transport Layer Security protocols persistently.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### TLS 1.1 and TLS 1.2

To apply persistent configuration of TLS 1.1 and TLS 1.2 to your PowerShell
ISE, you will have to define `[Net.ServicePointManager]::SecurityProtocol`
with the appropriate `[Net.SecurityProtocolType]::Tls11` (TLS 1.1) and
`[Net.SecurityProtocolType]::Tls12` (TLS 1.2) properties to a
`Microsoft.PowerShell_Profile.ps1` file.

> Note: It is highly recommended to exclude SSL 3.0 and TLS 1.0 usage if your
environment is up to date and only uses TLS 1.1 or TLS 1.2 Transport Layer
security protocols.

```powershell
@"
# Configure PowerShell Transport Layer Security Protocols
[Net.ServicePointManager]::SecurityProtocol = `
    [Net.SecurityProtocolType]::Tls11,
    [Net.SecurityProtocolType]::Tls12 ;
"@ | `
Out-File `
    -FilePath "$ENV:USERPROFILE\Documents\WindowsPowerShell\Microsoft.PowerShellISE_Profile.ps1" `
    -Append
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### SSL 3.0, TLS 1.0, TLS 1.1 and TLS 1.2

If you have to work with legacy Transport Layer Security protocols because your
internal environment have not been upgraded to compliant with the new standard
yet, you can still configure your PowerShell ISE to include those legacy
SSL 3.0 and TLS 1.0 Transport Layer Security protocols.

> Note: It is still highly recommended that you upgrade your environment to
support TLS 1.2 or TLS 1.1 and stop using SSL 3.0 and TLS 1.0.

```powershell
@"
# Configure PowerShell Transport Layer Security Protocols
[Net.ServicePointManager]::SecurityProtocol = `
    [Net.SecurityProtocolType]::Ssl3,
    [Net.SecurityProtocolType]::Tls,
    [Net.SecurityProtocolType]::Tls11,
    [Net.SecurityProtocolType]::Tls12 ;
"@ | `
Out-File `
    -FilePath "$ENV:USERPROFILE\Documents\WindowsPowerShell\Microsoft.PowerShellISE_Profile.ps1" `
    -Append
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Conclusion

That's all, folks. There is nothing wrong with your `Invoke-WebRequest`,
`Invoke-RestMethod` or PowerShell. It is just that technology has moved on and
those default (`Ssl3`, `Tls`) might not be default in technologies
surrounding us on this world tomorrow.

As for TLS 1.3 protocol, it is still a draft Transport Layer Security protocol
and therefore it is not implemented in .Net Framework yet since this blog is
posted.

If you happen to like what you just read in the blog post, feel free to share
the blog post to others and keep them informed.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## References

- [ServicePointManager Class](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager(v=vs.110).aspx)
- [ServicePointManager.SecurityProtocol Property](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.securityprotocol(v=vs.110).aspx)
- [SecurityProtocolType Enumeration](https://msdn.microsoft.com/en-us/library/system.net.securityprotocoltype(v=vs.110).aspx)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>
