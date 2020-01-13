---
layout: single
title: "Getting Debian distro environment on Windows for DevOps"
excerpt: "A guide on enabling Windows Subsystem for Linux (WSL) and get linux
distros like Debian distro instance for Windows 10 or Server using command line."
author: Ryen Tang
header:
  image: ./assets/images/banners/Windows-1280x200.png
date: 2018-09-04 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Windows
  - Linux
  - Debian
  - "Windows Subsystem for Linux"
  - "Ryen Tang"
---

Previously, I blogged about:
 - [Getting Ubuntu distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-Ubuntu-distro-environment-on-Windows-for-DevOps/),
 - [Getting openSUSE distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-openSUSE-distro-environment-on-Windows-for-DevOps/), and
 - [Getting SLES distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-SLES-distro-environment-on-Windows-for-DevOps/).

And since then, there are a few additional distros available for Windows
Subsystem for Linux (WSL) and is definitely encouraging to have more options in
Windows today.

Without further ado, I will share my experience on how you can obtain Debian
GNU/Linux distro for Windows Subsystem for Linux (WSL) and keep the context of
this blog post similar to all my previous other distros blog posts.

This walk-through demonstrates on how you can enable the Windows Subsystem for
Linux (WSL), maintain your linux distro environment and upgrade the linux
distro instance to stay ahead. Let's get started with command lines.

## Getting Started with Debian on Windows Subsystem for Linux (WSL)

In this blog post, I will be documenting the basic steps in getting Bash and
other common linux tools from Debian distro working on Windows 10 using Windows
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

## How to obtain Debian distro instance for Windows

Once you have verified that your current environment meets the pre-requisite
requirements and you have enabled the Windows Subsystem for Linux feature.

Let's get started with obtaining Debian distro instance with Windows
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

### Downloading Debian distro instance

In this example, we will use `Invoke-WebRequest` PowerShell cmdlet to download
the linux distro application package to your home folder.

```powershell
# Download Debian application for WSL
Invoke-WebRequest `
    -Uri "https://aka.ms/wsl-debian-gnulinux" `
    -OutFile "~\Debian9.appx" `
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
# Install the Debian 9 WSL application
Add-AppxPackage `
    -Path "~\Debian9.appx" ;
```

During your initial launch of Debian on Windows 10, you will be requested to
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
    -Path "~\Debian9.appx" `
    -NewName "Debian9.zip" ;

# Expand the compressed file to destination
Expand-Archive `
    -Path "~\Debian9.zip" `
    -DestinationPath "~\.wsl\distro\Debian" ;

# Launch the distro setup
Start-Process `
    -FilePath "~\.wsl\distro\Debian\debian.exe" ;
```

Once the `debian.exe` is running, the installation will begin and you will be
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

### How to update the Debian distro instance

When you switched into the linux distro for the first time, you will need to
use the linux distro's preferred package manager to update and upgrade those
installed packages. This is because most of the linux distro are shipped with
an empty/minimal package catalog.

For Debian, we will use `apt update` command to update the packages index and
perform `apt upgrade` to upgrade those packages based on the up to date
packages index. For more information about `apt`, you can refer to the
documentation [here](https://manpages.debian.org/stretch/apt/apt.8.en.html).

> Note: Microsoft does not maintain those linux distro and the linux distro
instance running on Windows Subsystem for Linux are not maintained by Windows
Updates.

```sh
# Update and upgrade Debian
sudo apt update && sudo apt upgrade
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

### How to validate Debian distro instance version

To check the Debian distro instance version, use the `cat` command on
`/etc/debian_version` file.

```sh
# Display Debian version
cat /etc/debian_version
```

By using `cat` command on `/etc/debian_version` file, you will get an output of
the Debian update point releases.

```text
9.5
```

If you are interested on the operating system release information, use the
`cat` command on `/etc/os-release` file.

```sh
# Display Debian release
cat /etc/os-release
```

And you will obtain the the operating system release information as below.

```text
PRETTY_NAME="Debian GNU/Linux 9 (stretch)"
NAME="Debian GNU/Linux"
VERSION_ID="9"
VERSION="9 (stretch)"
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
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

### How to perform an in-place upgrade to Debian 10 for testing

If you are still using Debian 9 distro instance and wanted to test
Debian 10, you can use `apt-get dist-upgrade` command to upgrade your
distro instance to Debian 10.

> Note: At the time of this blog post publication, Debian 10 (Codename: Buster)
is not officially released yet and is considered unstable distribution that do
not get
[security updates in a timely manner](https://www.debian.org/releases/sid/).

Firstly, check your current sources list by using `cat` command on the
`/etc/apt/sources.list` file.

```sh
# Check your current sources list
cat /etc/apt/sources.list
```

The `cat` command will returns the current configured sources below.

```text
deb http://deb.debian.org/debian stretch main
deb http://deb.debian.org/debian stretch-updates main
deb http://security.debian.org/debian-security/ stretch/updates main
```

Next, you will modified the current sources list file using the `sed` command
with the `-i` parameter to replace "stretch" with "buster".

```sh
# Modify the sources list
sudo sed -i 's/stretch/buster/g' /etc/apt/sources.list

# Review your modified sources list
cat /etc/apt/sources.list
```

Once you have modified the sources list, review your modified sources list file
and check that "stretch" has been replaced with "buster".

```text
deb http://deb.debian.org/debian buster main
deb http://deb.debian.org/debian buster-updates main
deb http://security.debian.org/debian-security/ buster/updates main
```

Finally, perform an `apt-get update` to update the package lists followed by
`apt-get upgrade` to install the packages and upgrade Debian 9 to Debian 10
distro using `apt-get dist-upgrade` command.

```sh
# Upgrade Debian 9 to Debian 10
sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade
```

Once the upgrade has completed, check the Debian version.

```sh
# Display Debian version
cat /etc/debian_version
```

You should get an output like below.

```text
buster/sid
```

Next, you can check the release information.

```sh
# Display Debian release
cat /etc/os-release
```

And operating system release information should contains the followings below.

```text
PRETTY_NAME="Debian GNU/Linux buster/sid"
NAME="Debian GNU/Linux"
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
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

That is all, folks.

Now, you can have a Unix-like computer operating system derived from GNU/Linux
operating system running on Windows Subsystem for Linux in Windows or try out
Debian 10 "Buster" for experimental purposes.

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

- [Microsoft Blogs: Debian GNU/Linux for WSL now available in the Windows Store](https://blogs.msdn.microsoft.com/commandline/2018/03/06/debian-gnulinux-for-wsl-now-available-in-the-windows-store/)
- [Microsoft Docs: Initializing a newly installed distro](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro)
- [Debian Wiki: Installing Debian On Microsoft Windows Subsystem For Linux](https://wiki.debian.org/InstallingDebianOn/Microsoft/Windows/SubsystemForLinux)
- [Debian Salsa: Debian App for WSL repository](https://salsa.debian.org/rhaist-guest/WSL)

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
