---
layout: single
title: "Getting openSUSE distro environment on Windows for DevOps"
excerpt: "A guide on enabling Windows Subsystem for Linux (WSL) and get linux
distros like openSUSE distro instance for Windows 10 or Server using command line."
author: Ryen Tang
header:
  image: ./assets/images/banners/Windows-1280x200.png
date: 2018-08-01 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Windows
  - Linux
  - openSUSE
  - "Windows Subsystem for Linux"
  - "Ryen Tang"
---

Previously, I blogged about
[Getting Ubuntu distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-Ubuntu-distro-environment-on-Windows-for-DevOps/)
and some may wonder if Ubuntu the only option for Windows Subsystem for Linux
(WSL) since there are tons of blog post about it.

As such, I will share my experience on how you can obtain openSUSE distro for
Windows Subsystem for Linux (WSL) and addresses the following questions below.

- Can I have other linux distro? 
- Can it be deployed on a Windows Server?
- Can it be upgraded so that it does not reach end of life?

I decided to blog this walk-through on how you can enable the Windows Subsystem
for Linux (WSL), maintain your linux distro environment and upgrade the linux
distro instance to stay ahead. Let's get started with command lines.

## Getting Started with openSUSE on Windows Subsystem for Linux (WSL)

In this blog post, I will be documenting the basic steps in getting Bash and
other common linux tools from openSUSE distro working on Windows 10 using Windows
Subsystem for Linux (WSL) feature.

What is actually Windows Subsystem for Linux? In short, it is a Windows feature
on Windows 10 that allows developers to run linux environment directly on
Windows without deploying a virtual machine. That means you get to use Bash
and many other tools that focus primarily on linux first to work on Windows.

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

## Pre-requisite requirements

- Windows 10 Build
[16215](https://docs.microsoft.com/en-us/windows/wsl/release-notes#build-16215)
or later
- Windows Server 1709 or
[later](https://docs.microsoft.com/en-us/windows-server/get-started/whats-new-in-windows-server-1803#windows-subsystem-for-linux-wsl)

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
    -FeatureName "Microsoft-Windows-Subsystem-Linux" `
    -Online `
    -NoRestart:$False ;
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

## How to obtain openSUSE distro instance for Windows

Once you have verified that your current environment meets the pre-requisite
requirements and you have enabled the Windows Subsystem for Linux feature.

Let's get started with obtaining openSUSE distro instance with Windows
Subsystem for Linux for Windows 10 or Windows Server.

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

### Downloading openSUSE distro instance

In this example, we will use `Invoke-WebRequest` PowerShell cmdlet to download
the linux distro application package to your home folder.

```powershell
# Download openSUSE WSL application
Invoke-WebRequest `
    -Uri "https://aka.ms/wsl-opensuse-42" `
    -OutFile "~\openSUSE-42_v1.appx" `
    -UseBasicParsing ;
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

### Installation for Windows 10

Natively, you can use `Add-AppxPackage` PowerShell cmdlet to add the linux
distro application package to your Windows 10.

```powershell
# Install the openSUSE Leap 42 WSL application
Add-AppxPackage `
    -Path "~\openSUSE-42_v1.appx" ;
```

During your initial launch of openSUSE on Windows 10, you will be requested to
configure your new UNIX username and password.

> Note: This setup a normal non-administrative user account that will
login by default when you launch the distro. The username and password does not
require to be same as your Windows user account. To elevate privileges in the
distro instance, use `sudo` and input your password. For more information, you
are refer to this documentation
[here](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro#setting-up-a-new-linux-user-account).

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
    -Path "~\openSUSE-42_v1.appx" `
    -NewName "openSUSE-42_v1.zip" ;

# Expand the compressed file to destination
Expand-Archive `
    -Path "~\openSUSE-42_v1.zip" `
    -DestinationPath "~\.wsl\distro\openSUSE" ;

# Launch the distro setup
Start-Process `
    -FilePath "~\.wsl\distro\openSUSE\openSUSE-42.exe" ;
```

Once the `openSUSE-42.exe` is running, the installation will begin and you will be
requested to configure your new UNIX username and password.

> Note: This setup a normal non-administrative user account that will
login by default when you launch the distro. The username and password does not
require to be same as your Windows user account. To elevate privileges in the
distro instance, use `sudo` and input your password. For more information, you
are refer to this documentation
[here](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro#setting-up-a-new-linux-user-account).

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

### How to update the openSUSE distro instance

When you switched into the linux distro for the first time, you will need to
use the linux distro's preferred package manager to update and upgrade those
installed packages. This is because most of the linux distro are shipped with
an empty/minimal package catalog.

For openSUSE, we will use `zypper removerepo` command to remove existing
repository uri and perform `zypper addrepo` command to add new repository uri
to the package manager configuration.

Once the package manager is configured properly, we will use `zypper refresh`
to initiate a refresh and apply the new packages using the `zypper update`
command. For more information about `zypper`, you can refer to the documentation
[here](https://doc.opensuse.org/documentation/leap/reference/html/book.opensuse.reference/cha.sw_cl.html).

> Note: Microsoft does not maintain those linux distro and the linux distro
instance running on Windows Subsystem for Linux are not maintained by Windows
Updates.

> Note: You can obtain openSUSE repository uri for the specific
[distribution](https://download.opensuse.org/distribution/) and
[update](https://download.opensuse.org/update/) releases from
[https://download.opensuse.org/](https://download.opensuse.org/).

```sh
# Remove old repository uri
sudo zypper removerepo oss oss_update

# Add openSUSE Leap 42.3 repository uri
zypper addrepo --check --refresh --name 'oss_42.3' http://download.opensuse.org/distribution/leap/42.3/repo/oss/suse/ oss
zypper addrepo --check --refresh --name 'oss_42.3_update' http://download.opensuse.org/update/leap/42.3/oss/ oss_update

# Refresh and update openSUSE
sudo zypper refresh && sudo zypper update
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

### How to switch into openSUSE distro shell environment

After you have configured and updated your openSUSE distro instance, you can
always quickly switch into the linux distro shell environment by doing the
following below.

- Launch Command Prompt or PowerShell Console
- Input `wsl` or `bash` to switch into openSUSE distro shell environment

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

### How to validate openSUSE distro instance version

To check the openSUSE distro instance version, use the `cat` command with `/etc/os-release` path.

```sh
# Display openSUSE version
cat /etc/os-release
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

### How to perform an in-place upgrade to openSUSE Leap 15.0

If you are still using openSUSE Leap 42.3 distro instance and wanted to use
openSUSE Leap 15.0, you can use `zypper removerepo` command to remove those old
repository uri and use `zypper addrepo` to add new repository uri to configure
the package manager.

After the package manager is configured, use `zypper refresh` to initiate a
refresh of the packages index and use `zypper dist-upgrade` to perform the
distro upgrade to openSUSE Leap 15.0.

```sh
# Remove old repository uri
sudo zypper removerepo oss oss_update

# Add openSUSE Leap 15.0 repository uri
zypper addrepo --check --refresh --name 'oss_15.0' http://download.opensuse.org/distribution/leap/15.0/repo/oss/ oss
zypper addrepo --check --refresh --name 'oss_15.0_update' http://download.opensuse.org/update/leap/15.0/oss/ oss_update

# Refresh and upgrade openSUSE
sudo zypper refresh && sudo zypper dist-upgrade
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

### How to update the openSUSE distro instance to openSUSE Leap 15.1

To update openSUSE Leap 15.0 to openSUSE Leap 15.1, we will use
`zypper removerepo` command to remove existing repository uri and perform
`zypper addrepo` command to add new repository uri to the package manager
configuration.

Once the package manager is configured properly, we will use `zypper refresh`
to initiate a refresh and apply the new packages using the `zypper update`
command. For more information about `zypper`, you can refer to the documentation
[here](https://doc.opensuse.org/documentation/leap/reference/html/book.opensuse.reference/cha.sw_cl.html).

> Note: openSUSE Leap 15.1 does not have an update repository uri at the time
when the blog post is published and therefore you will notice that we did not
include the step to use `zypper addrepo` to add `oss_15.1_update`
repository name.

```sh
# Remove old repository uri
sudo zypper removerepo oss oss_update

# Add openSUSE Leap 15.1 repository uri
zypper addrepo --check --refresh --name 'oss_15.1' http://download.opensuse.org/distribution/leap/15.1/repo/oss/ oss

# Refresh and update openSUSE
sudo zypper refresh && sudo zypper update
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

There you have it. You can deploy this feature on Windows 10 or Windows Server
and keep the instance up to date with that linux distro before it reaches end
of life.

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

## References

- [Micrsoft Blogs: SUSE’s Linux distros for WSL now available in the Windows Store](https://blogs.msdn.microsoft.com/commandline/2017/07/19/suses-linux-distros-for-wsl-now-available-in-the-windows-store/)
- [Microsoft Docs: Initializing a newly installed distro](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro)
- [openSUSE Wiki: openSUSE - Running the Upgrade](https://en.opensuse.org/SDB:System_upgrade)

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

<div id="amzn-assoc-ad-22001220-84c8-4c29-a8ca-d233867de335"></div><script async src="//z-na.amazon-adsystem.com/widgets/onejs?MarketPlace=US&adInstanceId=22001220-84c8-4c29-a8ca-d233867de335"></script>

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