---
layout: single
title: "The easy way to get Ubuntu 18.04 distro environment on Windows"
excerpt: "A guide on removing Ubuntu 16.04 and installing Ubuntu 18.04 to your
Windows using command lines without performing in-place upgrade of the distro
release."
author: Ryen Tang
header:
  image: ./assets/images/banners/Windows-1280x200.png
date: 2018-09-03 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Windows
  - Linux
  - Ubuntu
  - "Windows Subsystem for Linux"
  - "Ryen Tang"
---

Remember my previous
[Getting Ubuntu distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-Ubuntu-distro-environment-on-Windows-for-DevOps/)
blog post where I demonstrated on how to obtain Ubuntu 16.04 on Windows 10
or Windows Server and perform an in-place upgrade to Ubuntu 18.04?

Guess if you are a 100% pure Windows user and performing an in-place upgrade to
Ubuntu 18.04 in bash is not something you are comfortable with, you can start
uninstalling the Ubuntu 16.04 and install Ubuntu 18.04 again.

In this blog post, I will be providing a guide on how you can quickly get the
Ubuntu 16.04 replaced by Ubuntu 18.04 on your Windows. Let's get started with
the upgrade in command lines.

## A quick guide in getting Ubuntu 18.04 on Windows

This is a quick walk-through guide in removing Ubuntu 16.04 from Windows
Subsystem for Linux in Windows and install Ubuntu 18.04 onto Windows Subsystem
for Linux in Windows to refresh the Ubuntu distro environment to keep it up to
date.

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

### Uninstalling a Windows Subsystem for Linux distribution

To uninstall Ubuntu 16.04 from Windows Subsystem for Linux, use the `lxrun`
command with the `/uninstall` and `/full` parameters.

> Note: For Ubuntu 16.04 from Windows 10 Store, you can right click on the app
tile and select Uninstall.

```sh
lxrun /uninstall /full
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

### Ensuring Windows Subsystem for Linux feature is enabled

This section is to validate that Windows Subsystem for Linux feature is enabled
on Windows 10 or Windows Server and automatically enabling the feature if it is
found to be not enabled.

> Note: A reboot of the Windows operating system is required after enabling the
Windows Subsystem for Linux feature.

```powershell
# Check if Microsoft-Windows-Subsystem-Linux feature is enabled 
if((Get-WindowsOptionalFeature `
    -FeatureName "Microsoft-Windows-Subsystem-Linux" `
    -Online).State -ne "Enabled")
{
    # Enable Microsoft-Windows-Subsystem-Linux feature if
    #  the feature is not enabled
    Enable-WindowsOptionalFeature `
        -FeatureName "Microsoft-Windows-Subsystem-Linux" `
        -Online `
        -NoRestart:$False ;
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

### Downloading Ubuntu 18.04 distro instance

Once we have confirmed that requirement of Windows Subsystem for Linux feature
is enabled, we can initiate a download of Ubuntu 18.04 application.

> Note: You can rename the `-OutFile "<long file name>"` to
`-OutFile "~\Ubuntu1804.appx"` instead of the long file name.

```powershell
# Download Ubuntu application for WSL
Invoke-WebRequest `
    -Uri "https://aka.ms/wsl-ubuntu-1804" `
    -OutFile "~\CanonicalGroupLimited.Ubuntu18.04onWindows_1804.2018.817.0_x64__79rhkp1fndgsc.appx" `
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
# Install the Ubuntu application for WSL
Add-AppxPackage `
    -Path "~\CanonicalGroupLimited.Ubuntu18.04onWindows_1804.2018.817.0_x64__79rhkp1fndgsc.appx" ;
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
    -Path "~\CanonicalGroupLimited.Ubuntu18.04onWindows_1804.2018.817.0_x64__79rhkp1fndgsc.appx" `
    -NewName "Ubuntu1804.zip" ;

# Expand the compressed file to destination
Expand-Archive `
    -Path "~\Ubuntu1804.zip" `
    -DestinationPath "~\.wsl\distro\Ubuntu" ;

# Launch the distro setup
Start-Process `
    -FilePath "~\.wsl\distro\Ubuntu\Ubuntu1804.exe" ;
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

### Updating the Ubuntu 18.04 distro instance

Finally, you will have to switch into the Ubuntu 18.04 bash and perform an
update of the packages to keep up to date.

```sh
# Update and upgrade Ubuntu
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

## Conclusion

That's all on how to obtain Ubuntu 18.04 on Windows Subsystem for Linux and
stay up to date with Ubuntu releases for any Windows administrators out there
that does not want to meddle with any Linux package manager.

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

- [How do I uninstall a WSL Distribution](https://docs.microsoft.com/en-us/windows/wsl/faq#how-do-i-uninstall-a-wsl-distribution)
- [Getting Ubuntu distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-Ubuntu-distro-environment-on-Windows-for-DevOps/)

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