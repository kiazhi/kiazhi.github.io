---
layout: single
title: "Getting Kali distro environment on Windows for SecOps or DevOps"
excerpt: "A guide on enabling Windows Subsystem for Linux (WSL) and get Kali
distro instance for Windows 10 or Server using command line for Security
enthusiasts."
author: Ryen Tang
header:
  image: ./assets/images/banners/Windows-1280x200.png
date: 2018-09-06 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Windows
  - Linux
  - Kali
  - "Windows Subsystem for Linux"
  - "Ryen Tang"
---

Previously, I blogged about:
 - [Getting Ubuntu distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-Ubuntu-distro-environment-on-Windows-for-DevOps/),
 - [Getting openSUSE distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-openSUSE-distro-environment-on-Windows-for-DevOps/),
 - [Getting SLES distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-SLES-distro-environment-on-Windows-for-DevOps/), and
 - [Getting Debian distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-Debian-distro-environment-on-Windows-for-DevOps/)

Today, I will be sharing an additional distro for Windows Subsystem for Linux
(WSL) that targets SecOps more than DevOps and most likely will be my favourite
distro that will be a permanent resident on my Windows.

Let me introduce to you, [Kali](https://www.kali.org/) Linux a Debian-derived
linux distribution primarily for hacking and penetration testing. And probably
one of the most advanced penetration testing distribution, ever.

This walk-through demonstrates on how you can enable the Windows Subsystem for
Linux (WSL), maintain your linux distro environment and upgrade the linux
distro instance to stay ahead. Let's get started with command lines.

## Getting Started with Kali on Windows Subsystem for Linux (WSL)

In this blog post, I will be documenting the basic steps in getting Bash and
other common linux tools from Kali distro working on Windows 10 using Windows
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

## How to obtain Kali distro instance for Windows

Once you have verified that your current environment meets the pre-requisite
requirements and you have enabled the Windows Subsystem for Linux feature.

Let's get started with obtaining Kali distro instance with Windows
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

### Downloading Kali distro instance

In this example, we will use `Invoke-WebRequest` PowerShell cmdlet to download
the linux distro application package to your home folder.

```powershell
# Download Kali application for WSL
Invoke-WebRequest `
    -Uri "https://aka.ms/wsl-kali-linux" `
    -OutFile "~\Kali.appx" `
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
# Install the Kali 2018.3 WSL application
Add-AppxPackage `
    -Path "~\Kali.appx" ;
```

During your initial launch of Kali on Windows 10, you will be requested to
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

In this particular Kali distro package release, the process is slightly
different from the other linux distro packages because it is packaged
differently and you will immediately notice the differences in the compressed
file content structure.

In order install into Windows Server, you will need to repeat the use of
`Rename-Item` to rename the application package extension and execute
`Expand-Archive` to uncompress the file process twice.

To start of on Windows Server, you will have use `Rename-Item` PowerShell
cmdlet to rename the linux distro application package extension to a compressed
file extension.

After the file has been renamed to a compressed file extension, you will use
`Expand-Archive` PowerShell cmdlet to expand the compressed file to your
temporary folder or `~\AppData\Local\Temp\Kali` user temporary folder location.

Next, you will have use `Rename-Item` PowerShell cmdlet to rename the
`DistroLauncher-Appx_1.1.4.0_x64.appx` application package extension to a
compressed file extension again and use `Expand-Archive` PowerShell cmdlet to
expand the compressed file to your home folder or `~\.wsl\distro\` custom home
folder location.

After the file has been expanded to the destination, use the `Start-Process`
PowerShell cmdlet to launch the executable to begin the initial configuration
of the linux distro instance.

```powershell
# Rename the file extension to compressed file extension
Rename-Item `
    -Path "~\Kali.appx" `
    -NewName "Kali.zip" ;

# Expand the compressed file to destination
Expand-Archive `
    -Path "~\Kali.zip" `
    -DestinationPath "~\AppData\Local\Temp\Kali" ;

Rename-Item `
    -Path "~\AppData\Local\Temp\Kali\DistroLauncher-Appx_1.1.4.0_x64.appx" `
    -NewName "Kali.zip" ;

# Expand the compressed file to destination
Expand-Archive `
    -Path "~\AppData\Local\Temp\Kali\Kali.zip" `
    -DestinationPath "~\.wsl\distro\Kali" ;

# Launch the distro setup
Start-Process `
    -FilePath "~\.wsl\distro\Kali\kali.exe" ;
```

Once the `kali.exe` is running, the installation will begin and you will be
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

### How to update the Kali distro instance

When you switched into the linux distro for the first time, you will need to
use the linux distro's preferred package manager to update and upgrade those
installed packages. This is because most of the linux distro are shipped with
an empty/minimal package catalog.

For Debian-derived Kali Linux distribution, we will use `apt update` command to
update the packages index and perform `apt upgrade` to upgrade those packages
based on the up to date packages index. For more information about `apt`, you
can refer to the documentation
[here](https://manpages.debian.org/stretch/apt/apt.8.en.html).

> Note: Microsoft does not maintain those linux distro and the linux distro
instance running on Windows Subsystem for Linux are not maintained by Windows
Updates.

```sh
# Update and upgrade Kali
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

### How to validate Kali distro instance version

Because Kali Linux is a Debian-derived Linux distribution, you can check the
Kali distro instance version, use the `cat` command on `/etc/debian_version`
file.

```sh
# Display Kali version
cat /etc/debian_version
```

By using `cat` command on `/etc/debian_version` file, you will get an output of
the Kali release name.

```text
kali-rolling
```

If you are interested on the operating system release information, use the
`cat` command on `/etc/os-release` file.

```sh
# Display Kali release
cat /etc/os-release
```

And you will obtain the the operating system release information as below.

```text
PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
ID=kali
VERSION="2018.3"
VERSION_ID="2018.3"
ID_LIKE=debian
ANSI_COLOR="1;31"
HOME_URL="https://www.kali.org/"
SUPPORT_URL="https://forums.kali.org/"
BUG_REPORT_URL="https://bugs.kali.org/"
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

### How to perform an in-place upgrade of Kali

If you are still using Kali 2018.2 distro instance and wanted to test
Kali 2018.3, you can use `apt-get dist-upgrade` command to upgrade your
distro instance to Kali.

> Note: At the time of this blog post publication, Kali Linux 2018.3 is the
latest release.

Firstly, check your current sources list by using `cat` command on the
`/etc/apt/sources.list` file.

```sh
# Check your current sources list
cat /etc/apt/sources.list
```

The `cat` command will returns the current configured sources and validate the
source list contains the following below.

```text
deb http://http.kali.org/kali kali-rolling main non-free contrib
```

Finally, perform an `apt update` to update the package lists followed by
`apt full-upgrade` to install the packages and upgrade Kali older rolling
version to Kali latest rolling version.

```sh
# Upgrade Kali older release to Kali 2018.3
sudo apt update && sudo apt -y full-upgrade
```

Once the upgrade has completed, check the Kali version.

```sh
# Display Kali version
cat /etc/debian_version
```

You should get an output like below.

```text
kali-rolling
```

Next, you can check the release information.

```sh
# Display Kali release
cat /etc/os-release
```

And operating system release information should contains the followings below.

```text
PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
ID=kali
VERSION="2018.3"
VERSION_ID="2018.3"
ID_LIKE=debian
ANSI_COLOR="1;31"
HOME_URL="https://www.kali.org/"
SUPPORT_URL="https://forums.kali.org/"
BUG_REPORT_URL="https://bugs.kali.org/"
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

This definitely a lovely addition to Windows Subsystem for Linux (WSL) for
those security folks out there.

Now, you can have a Kali Linux derived from Debian GNU/Linux
operating system running on Windows Subsystem for Linux in Windows for hacking
and penetration testing tool in your IT security arsenal.

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

- [Microsoft Blogs: Kali Linux for WSL now available in the Windows Store](https://blogs.msdn.microsoft.com/commandline/2018/03/05/kali-linux-for-wsl/)
- [Microsoft Docs: Initializing a newly installed distro](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro)
- [Kali News: Kali Linux in the Windows App Store](https://www.kali.org/news/kali-linux-in-the-windows-app-store/)
- [Kali Git: Live Build repository](http://git.kali.org/gitweb/?p=live-build-config.git)

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