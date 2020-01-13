---
layout: single
title: "Introducing PowerShell in Oracle Linux container"
excerpt: "Do you want to introduce PowerShell into your Oracle Linux container?
Find out how you can install PowerShell into your Oracle Linux from here for
Oracle DBAs."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-11-14 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - Container
  - Linux
  - "Oracle Linux"
  - PowerShell
  - "Ryen Tang"
---

With my previous blog posts about installing PowerShell on the following Linux
distros:

- [Kali Linux](https://kiazhi.github.io/blog/powershell/Getting-PowerShell-in-Kali-Linux-container/)
- [Parrot Linux](https://kiazhi.github.io/blog/powershell/Needing-PowerShell-in-Parrot-Linux-container/)
- [Arch Linux](https://kiazhi.github.io/blog/powershell/Looking-for-PowerShell-in-Arch-Linux-container/)
- [BlackArch Linux](https://kiazhi.github.io/blog/powershell/Having-PowerShell-in-BlackArch-container/)
- [Amazon Linux](https://kiazhi.github.io/blog/powershell/Adding-PowerShell-in-Amazon-Linux-container/)

I will be demonstrating on how you can install PowerShell 6 in
[Oracle Linux](https://www.oracle.com/linux/) distro container.

Like my previous blog post, I will not be providing a break down of the steps
in the `Dockerfile` with explainations except including comments in the
`Dockerfile` to indicate what it is doing.

## PowerShell for Oracle Linux container

In this section, I will provide a `Dockerfile` text document file that contains
all the commands to build a Oracle Linux container and install PowerShell to
the Oracle Linux distro when you use DockerCLI `docker build` command.

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

### Creating the Dockerfile

Let us create a new `Dockerfile` text document file to begin with.
Next, copy the code below and paste it into that `Dockerfile` text document
file.
Remember to save the `Dockerfile` file and exit from your editor.

```dockerfile
# Pull Oracle Linux container image
FROM oraclelinux:latest AS build

# Define Args and Env needed to create links
ENV PS_INSTALL_FOLDER=/opt/microsoft/powershell/6 \
    # Define ENVs for Localization/Globalization
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

# Download the PowerShell RPM package
ADD https://github.com/PowerShell/PowerShell/releases/download/v6.1.0/powershell-6.1.0-1.rhel.7.x86_64.rpm \
    /tmp/powershell-linux.rpm

# Installation and configuration
RUN \
    # Install dependencies
    yum install -y \
     # Required for help in powershell
     less \
    # Install powershell package
    && yum install -y /tmp/powershell-linux.rpm \
    # Remove powershell package
    && rm -f /tmp/powershell-linux.rpm \
    # Upgrade packages
    && yum upgrade -y \
    # Clean cached data
    && yum clean all \
    # Remove cache folders and files
    && rm -rf /var/cache/yum

# Configure entrypoint for the container
ENTRYPOINT ["pwsh", "-c"]

# Configure PowerShell as default shell for the container
CMD ["pwsh"]
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

### Building from the Dockerfile

Now, you will have to use the DockerCLI `docker build` command to build the
Oracle Linux container from the instructions in the `Dockerfile` text
document file.

```sh
# Change to your working directory
cd \The\directory\where\your\Dockerfile\is\saved

# Build the container with the Dockerfile
docker build -t oraclelinux .
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

Once the container build has completed, you can use the DockerCLI `docker run`
command to run the container and look at PowerShell instead of Bash.

```sh
docker run --rm --interactive oraclelinux
```

And if you need to go back into Bash from the `pwsh` in the container,
type `bin/bash` to switch into Bash.

But if you requires to run the container in `bash`, you can do it on the same
container with the command below.

```sh
docker run --rm --interactive --tty oraclelinux bin/bash
```

And if you need to go back into PowerShell from the `bash` in the container,
type `pwsh` to switch into PowerShell.

If you happen to be having an Oracle database in the Oracle Linux container and
is interested in managing it using PowerShell, you can try out
[JS.OracleDatabase](https://www.powershellgallery.com/packages/JS.OracleDatabase)
PowerShell module written by
[Jesus Alejandro Sanchez Davila](https://community.oracle.com/people/Jesus%20Sanchez)
with his blog post related to this module,
[here](https://community.oracle.com/blogs/jsanchez_tips/2017/12/13/using-powershell-and-the-oracle-client-to-automate).

```powershell
# Install JS.OracleDatabase PowerShell module
# from PowerShell Gallery
Install-Module `
     -Name "JS.OracleDatabase" `
     -Force

# Import the module
Import-Module `
     -Name "JS.OracleDatabase"

# Get a list of commands available from the module
Get-Command `
     -Module "JS.OracleDatabase"
```

That is it on how to get PowerShell Core 6 in this distro for Oracle Linux
folks.

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

- [Docker Docs - Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
- [Microsoft Docs - Installing PowerShell Core on Linux](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-linux)
- [Oracle Docs - Oracle Linux 7](https://docs.oracle.com/cd/E52668_01/)
- [Oracle Community Blogs - Using PowerShell and the Oracle Client to Automate](https://community.oracle.com/blogs/jsanchez_tips/2017/12/13/using-powershell-and-the-oracle-client-to-automate)
- [PowerShell Gallery - JS.OracleDatabase](https://www.powershellgallery.com/packages/JS.OracleDatabase)

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

<div id="amzn-assoc-ad-a810e3df-f462-4f55-be29-a78a4507d7bf"></div><script async src="//z-na.amazon-adsystem.com/widgets/onejs?MarketPlace=US&adInstanceId=a810e3df-f462-4f55-be29-a78a4507d7bf"></script>

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
