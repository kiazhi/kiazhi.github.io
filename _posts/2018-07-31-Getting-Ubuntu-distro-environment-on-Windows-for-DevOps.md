---
layout: single
title: "Getting Ubuntu distro environment on Windows for DevOps"
excerpt: "A guide on enabling Windows Subsystem for Linux (WSL) and get linux
distros like Ubuntu distro instance for Windows 10 or Server using command line."
author: Ryen Tang
header:
  image: ./assets/images/banners/Windows-1280x200.png
date: 2018-07-31 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Windows
  - "Windows Subsystem for Linux"
  - "Ryen Tang"
---

Previously, I blogged about
[Working with Ruby and Jekyll on Windows for GitHub Pages](https://kiazhi.github.io/blog/Working-with-Jekyll-and-Ruby-on-Windows-for-GitHub-Pages/)
and I am constantly being asked about Windows Subsystem for Linux (WSL).

- Can it be deployed on a Windows Server?
- Can it be upgraded so that it does not reach end of life?

I decided to blog this walk-through on how you can enable the Windows Subsystem
for Linux (WSL), maintain your linux distro environment and upgrade the linux
distro instance to stay ahead. Let's get started with command lines.

## Getting Started with Ubuntu on Windows Subsystem for Linux (WSL)

In this blog post, I will be documenting the basic steps in getting Bash and
other common linux tools from Ubuntu distro working on Windows 10 using Windows
Subsystem for Linux (WSL) feature.

What is actually Windows Subsystem for Linux? In short, it is a Windows feature
on Windows 10 that allows developers to run linux environment directly on
Windows without deploying a virtual machine. That means you get to use Bash
and many other tools that focus primarily on linux first to work on Windows.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Pre-requisite requirements

- Windows 10 Build
[16215](https://docs.microsoft.com/en-us/windows/wsl/release-notes#build-16215)
or later
- Windows Server 1709 or
[later](https://docs.microsoft.com/en-us/windows-server/get-started/whats-new-in-windows-server-1803#windows-subsystem-for-linux-wsl)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Enable Windows Subsystem for Linux feature on Windows 10

To enable the Windows Subsystem for Linux Feature on Windows 10 and reboot the
Windows 10 using PowerShell.

> Note: A reboot of the Windows 10 operating system is required after enabling
the Windows Subsystem for Linux feature

- Launch Windows PowerShell with elevated privileges
- Use the `Enable-WindowsOptionalFeature` PowerShell cmdlet to enable the
feature

```powershell
Enable-WindowsOptionalFeature `
    -FeatureName Microsoft-Windows-Subsystem-Linux `
    -Online `
    -NoRestart:$False ;
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## How to obtain Ubuntu distro instance for Windows

Once you have verified that your current environment meets the pre-requisite
requirements and you have enabled the Windows Subsystem for Linux feature.

Let's get started with obtaining Ubuntu distro instance with Windows Subsystem
for Linux for Windows 10 or Windows Server.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Downloading Ubuntu distro instance

In this example, we will use `Invoke-WebRequest` PowerShell cmdlet to download
the linux distro application package to your home folder.

```powershell
# Download Ubuntu application for WSL
Invoke-WebRequest `
    -Uri https://aka.ms/wsl-ubuntu `
    -OutFile ~\Ubuntu.appx `
    -UseBasicParsing ;
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Installation for Windows 10

Natively, you can use `Add-AppxPackage` PowerShell cmdlet to add the linux
distro application package to your Windows 10.

```powershell
# Install the Ubuntu application for WSL
Add-AppxPackage `
    -Path ~/Ubuntu.appx ;
```

During your initial launch of Ubuntu on Windows 10, you will be requested to
configure your new UNIX username and password.

> Note: This setup a normal non-administrative user account that will
login by default when you launch the distro. The username and password does not
require to be same as your Windows user account. To elevate privileges in the
distro instance, use `sudo` and input your password. For more information, you
are refer to this documentation
[here](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro#setting-up-a-new-linux-user-account).

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Installation for Windows Server

For Windows Server, you will have use `Rename-Item` PowerShell cmdlet to rename
the linux distro application package extension to a compressed file extension.

Since the file has been renamed to a compressed file extension, you will use
`Expand-Archive` PowerShell cmdlet to expand the compressed file to your home
folder or `~\.wsl\distro\` custom home folder location.

After the file has been expanded to the destination, use the `Start-Process`
PowerShell cmdlet to launch the executable to begin the initial configuration
of the linux distro instance.

```powershell
# Rename the file extension to compressed file extension
Rename-Item `
    -Path ~\Ubuntu.appx `
    -NewName Ubuntu.zip ;

# Expand the compressed file to destination
Expand-Archive `
    -Path ~\Ubuntu.zip `
    -DestinationPath ~\.wsl\distro\Ubuntu ;

# Launch the distro setup
Start-Process `
    -FilePath ~\.wsl\distro\Ubuntu\Ubuntu.exe ;
```

Once the `Ubuntu.exe` is running, the installation will begin and you will be
requested to configure your new UNIX username and password.

> Note: This setup a normal non-administrative user account that will
login by default when you launch the distro. The username and password does not
require to be same as your Windows user account. To elevate privileges in the
distro instance, use `sudo` and input your password. For more information, you
are refer to this documentation
[here](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro#setting-up-a-new-linux-user-account).

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### How to update the Ubuntu distro instance

When you switched into the linux distro for the first time, you will need to
use the linux distro's preferred package manager to update and upgrade those
installed packages. This is because most of the linux distro are shipped with
an empty/minimal package catalog.

For Ubuntu, we will use `apt update` command to update the packages index and
perform `apt upgrade` to upgrade those packages based on the up to date
packages index. For more information about `apt`, you can refer to the
documentation [here](https://help.ubuntu.com/lts/serverguide/apt.html.en).

> Note: Microsoft does not maintain those linux distro and the linux distro
instance running on Windows Subsystem for Linux are not maintained by Windows
Updates.

```sh
# Update and upgrade Ubuntu
sudo apt update && sudo apt upgrade
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### How to validate Ubuntu distro instance version

To check the Ubuntu distro instance version, use the `lsb_release` command with `-a` parameter.

```sh
# Display Ubuntu version
lsb_release -a
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### How to perform an in-place upgrade to Ubuntu 18.04 LTS

If you are still using Ubuntu 16.04 LTS distro instance and wanted to use
Ubuntu 18.04 LTS, you can use `do-release-upgrade` command with `-d` parameter
to upgrade your distro instance to Ubuntu 18.04 LTS.

```sh
# Validate release upgrade configuration is set to LTS
cat /etc/update-manager/release-upgrades | grep Prompt=

# Upgrade Ubuntu 16.04 LTS to 18.04 LTS
sudo do-release-upgrade -d
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Conclusion

There you have it. You can deploy this feature on Windows 10 or Windows Server
and keep the instance up to date with that linux distro before it reaches end
of life.

If you find that this information is useful, feel free to bookmark this or
share it with your colleague.

## References

- [A Guide to Upgrading your Ubuntu App’s Release](https://blogs.msdn.microsoft.com/commandline/2018/07/09/upgrading-ubuntu/)
- [Initializing a newly installed distro](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>
