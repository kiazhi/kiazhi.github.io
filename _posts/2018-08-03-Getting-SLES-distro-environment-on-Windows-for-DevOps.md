---
layout: single
title: "Getting SLES distro environment on Windows for DevOps"
excerpt: "A guide on enabling Windows Subsystem for Linux (WSL) and get linux
distros like SLES distro instance for Windows 10 or Server using command line."
author: Ryen Tang
header:
  image: ./assets/images/banners/Windows-1280x200.png
date: 2018-08-03 00:00:00 +1200
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
[Getting Ubuntu distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-Ubuntu-distro-environment-on-Windows-for-DevOps/),
[Getting openSUSE distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-openSUSE-distro-environment-on-Windows-for-DevOps/)
and some may wonder if Ubuntu or openSUSE the only options for Windows
Subsystem for Linux (WSL).

As such, I will share my experience on how you can obtain SUSE Linux Enterprise
Server (SLES) distro for Windows Subsystem for Linux (WSL) and addresses the
following questions below.

- Can I have other linux distro?
- Can it be deployed on a Windows Server?
- Can it be upgraded so that it does not reach end of life?

I decided to blog this walk-through on how you can enable the Windows Subsystem
for Linux (WSL), maintain your linux distro environment and upgrade the linux
distro instance to stay ahead. Let's get started with command lines.

## Getting Started with SLES on Windows Subsystem for Linux (WSL)

In this blog post, I will be documenting the basic steps in getting Bash and
other common linux tools from SLES distro working on Windows 10 using Windows
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
    -FeatureName Microsoft-Windows-Subsystem-Linux `
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

## How to obtain SLES distro instance for Windows

Once you have verified that your current environment meets the pre-requisite
requirements and you have enabled the Windows Subsystem for Linux feature.

Let's get started with obtaining SLES distro instance with Windows
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

### Downloading SLES distro instance

In this example, we will use `Invoke-WebRequest` PowerShell cmdlet to download
the linux distro application package to your home folder.

```powershell
# Download SUSE Linux Enterprise Server 12 WSL application
Invoke-WebRequest `
    -Uri https://aka.ms/wsl-sles-12 `
    -OutFile ~\SLES-12_v1.appx `
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
# Install the SUSE Linux Enterprise Server 12 WSL application
Add-AppxPackage `
    -Path ~\SLES-12_v1.appx ;
```

During your initial launch of SUSE Enterprise Linux Server (SLES) on Windows
10, you will be requested to configure your new UNIX username and password.

> Note: You will also be prompted to input your SUSE Enterprise Linux Server
(SLES) registration code. If you have the registration code, you can input them
and get it registered. If not, you can leave it blank and skip the registration
for now. You can always enter the registration code later by using the
`SUSEConnect -r` command.

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
    -Path ~\SLES-12_v1.appx `
    -NewName SLES-12_v1.zip ;

# Expand the compressed file to destination
Expand-Archive `
    -Path ~\SLES-12_v1.zip `
    -DestinationPath ~\.wsl\distro\SLES ;

# Launch the distro setup
Start-Process `
    -FilePath ~\.wsl\distro\SLES\SLES-12.exe ;
```

Once the `SLES-12.exe` is running, the installation will begin and you will be
requested to configure your new UNIX username and password.

> Note: You will also be prompted to input your SUSE Enterprise Linux Server
(SLES) registration code. If you have the registration code, you can input them
and get it registered. If not, you can leave it blank and skip the registration
for now. You can always enter the registration code later by using the
`SUSEConnect -r` command.

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

### How to update the SLES distro instance

When you switched into the linux distro for the first time, you will need to
use the linux distro's preferred package manager to update and upgrade those
installed packages. This is because most of the linux distro are shipped with
an empty/minimal package catalog.

For SUSE Enterprise Linux Server (SLES), we will use
`SUSEConnect --regcode $YOUR_REGISTRATION_CODE`
command to register our registration code to obtain the SLES 12.2 repository
uri and perform `zypper patch --no-confirm` command to patch the current SLES
12.2 operating system.

Once the current SLES 12.2 is fully patched, you will use
`zypper install --no-confirm zypper-migration-plugin` command to install the
migration plugin package.

Lastly, you will use `zypper migration` command to migrate the current SLES
12.2 to SLES 12.3 version.

> Note: Microsoft does not maintain those linux distro and the linux distro
instance running on Windows Subsystem for Linux are not maintained by Windows
Updates.

> Note: To update SUSE Enterprise Linux Server (SLES), you will need to
register your SLES in order to obtain updates and you can obtain a free
developer subscription for 365 days by registering yourself from
[here](https://www.suse.com/subscriptions/sles/developer/) to obtain the
registration code.

```sh
# Add your SUSE Enterprise Linux Server (SLES) registration code
sudo SUSEConnect --regcode $YOUR_REGISTRATION_CODE

# Install the latest patches
sudo zypper patch --no-confirm

# Install the zypper-migration-plugin package and dependencies
sudo zypper install --no-confirm zypper-migration-plugin

# Start the migration to update SUSE Enterprise Linux Server (SLES)
sudo zypper migration
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

### How to switch into SLES distro shell environment

After you have configured and updated your SLES distro instance, you can
always quickly switch into the linux distro shell environment by doing the
following below.

- Launch Command Prompt or PowerShell Console
- Input `wsl` or `bash` to switch into SLES distro shell environment

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

### How to validate SLES distro instance version

To check the SLES distro instance version, use the `cat` command with `/etc/os-release` path.

```sh
# Display SLES version
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

### How to perform an in-place upgrade to SLES 15.0

If you are still using SLES 12.3 distro instance and wanted to use SLES 15.0,
you can use `SUSEConnect --cleanup` command to remove those old repository uri
and use `SUSEConnect --regcode $YOUR_REGISTRATION_CODE -p SLES/15.0/x86_64` to
add new repository uri to the package manager.

After the package manager is configured, use
`SUSEConnect -p sle-module-basesystem/15.0/x86_64` to select the base system
module from the extensions list and use `zypper dist-upgrade --force-resolution`
to perform the distro upgrade to SLES 15.0.

Lastly, remove those orphaned packages if you preferred to do so.

```sh
# Remove all previous registrations
sudo SUSEConnect --cleanup

# Register SUSE Linux Enterprise Server (SLES) 15.0 repositories
sudo SUSEConnect --regcode $YOUR_REGISTRATION_CODE -p SLES/15.0/x86_64

# Select SUSE Linux Enterprise Server (SLES) 15.0 base system module
sudo SUSEConnect -p sle-module-basesystem/15.0/x86_64

# Installed packages
sudo zypper dist-upgrade --force-resolution

# Remove orphaned packages
sudo zypper rm $(zypper --no-refresh packages --orphaned | gawk '{print $5}' | tail -n +5)
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

- [Initializing a newly installed distro](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro)
- [SLES 12 - Upgrading with Zypper](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_update_migr_zypper_onlinemigr.html)
- [SLES 15 - Upgrade Guide](https://www.suse.com/documentation/sles-15/singlehtml/book_sle_upgrade/#sec.upgrade-online.opensuse_to_sle)

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