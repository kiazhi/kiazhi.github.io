---
layout: single
title: "Obtaining PowerShell in Photon Linux container"
excerpt: "Need PowerShell to manage VMware vSphere in a Photon OS container
hosted in vSphere Integrated Containers platform? Find out how you can do this."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-12-04 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - Container
  - Linux
  - Photon
  - VMware
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
- [Oracle Linux](https://kiazhi.github.io/blog/powershell/Introducing-PowerShell-in-Oracle-Linux-container/)
- [Clear Linux](https://kiazhi.github.io/blog/powershell/Bringing-PowerShell-to-Clear-Linux-container/)

I will be demonstrating on how you can install PowerShell 6 in
[Photon](https://vmware.github.io/photon/) distro container.

Like my previous blog post, I will not be providing a break down of the steps
in the `Dockerfile` with explainations except including comments in the
`Dockerfile` to indicate what it is doing.

## PowerShell for Photon Linux container

In this section, I will provide a `Dockerfile` text document file that contains
all the commands to build a Photon Linux container and install PowerShell to
the Photon Linux distro when you use DockerCLI `docker build` command.

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
# Pull Photon Linux container image
FROM photon:2.0-20181017 AS stage

# Define Args and Env needed to create links
ENV PS_INSTALL_FOLDER=/opt/microsoft/powershell/6 \
    # Define ENVs for Localization/Globalization
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

# Download the PowerShell binary package
ADD https://github.com/PowerShell/PowerShell/releases/download/v6.1.0/powershell-6.1.0-linux-x64.tar.gz \
    /tmp/powershell-linux.tar.gz

# Installation and configuration
RUN \
    # update package list
    tdnf distro-sync \
    # install dependencies
    && tdnf -y install \
      gzip \
      tar \
    # create powershell folder
    && mkdir -p ${PS_INSTALL_FOLDER} \
    # uncompress powershell linux tar file
    && tar zxf /tmp/powershell-linux.tar.gz -C ${PS_INSTALL_FOLDER}

# Start a new stage so we lose all the tar.gz layers from the final image
FROM photon:2.0-20181017 as build

# Copy only the files we need from the previous stage
COPY --from=stage ["/opt/microsoft/powershell", "/opt/microsoft/powershell"]

# Define Args and Env needed to create links
ARG PS_INSTALL_VERSION=6
ENV PS_INSTALL_FOLDER=/opt/microsoft/powershell/$PS_INSTALL_VERSION \
    \
    # Define ENVs for Localization/Globalization
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    # Opt out of SocketsHttpHandler in DotNet Core 2.1 to use HttpClientHandler
    DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER=0 \
    # Set terminal to linux because there is no xterm library in Photon image
    # when docker allocate pseudo-tty
    TERM=linux

RUN \
    # update package list
    tdnf distro-sync \
    # install dependencies
    && tdnf -y install \
      # required for locales
      glibc-i18n \
      # required for uncompressing locale files in /usr/share/i18n/charmaps/
      gzip \
      # required for International Components for Unicode
      icu \
      # required for help in powershell
      less \
    # generate locale
    && locale-gen.sh \
    # create the pwsh symbolic link that points to powershell
    && ln -s ${PS_INSTALL_FOLDER}/pwsh /usr/bin/pwsh \
    # upgrade packages
    && tdnf -y upgrade \
    # clean cached data
    && tdnf clean all

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
Photon Linux container from the instructions in the `Dockerfile` text
document file.

```sh
# Change to your working directory
cd \The\directory\where\your\Dockerfile\is\saved

# Build the container with the Dockerfile
docker build -t photon .
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
docker run --rm --interactive photon
```

And if you need to go back into Bash from the `pwsh` in the container,
type `bin/bash` to switch into Bash.

But if you requires to run the container in `bash`, you can do it on the same
container with the command below.

```sh
docker run --rm --interactive --tty photon bin/bash
```

And if you need to go back into PowerShell from the `bash` in the container,
type `pwsh` to switch into PowerShell.

Now if you happen to have VMware vSphere Integrated Containers environment and
is interested in deploying VMware Photon OS container with PowerShell Core 6 to
manage your VMware vSphere environment using PowerShell, you can try installing
[VMware.PowerCLI](https://www.powershellgallery.com/packages/VMware.PowerCLI)
PowerShell module from [PowerShell Gallery](https://www.powershellgallery.com).

After installing `VMware.PowerCLI` module in the Photon container you created
just now, you can start managing that environment like this example below.

```powershell
# Install VMware.PowerCLI PowerShell module
Install-Module `
     -Name 'VMware.PowerCLI' `
     -Scope 'CurrentUser' `
     -Force

# Import the VMware.VimAutomation.Core PowerShell module
Import-Module `
     -Name 'VMware.VimAutomation.Core'

# Get a list of commands available from the module
Get-VICommand

# Define your VMware vSphere variables
$script:Hostname = 'vsphere.myorganization.com'
$script:Username = 'admin@myorganization.com'
$script:Password = 'MyPassw0rd'

# Connect to VMware vSphere
Connect-VIServer `
     -Server $script:Hostname `
     -Credential (New-Object `
          -TypeName 'System.Management.Automation.PSCredential' `
          -ArgumentList `
               $script:Username, `
               (ConvertTo-SecureString `
                    -String $script:Password `
                    -AsPlainText `
                    -Force))
```

Once you are connected to your vSphere, you will get a similar output below.

```text
Name                           Port  User
----                           ----  ----
vsphere.myorganization.com     443   myorganization\admin
```

And you can manage your VMware vSphere environment in PowerShell Core 6 from a
dedicated VMware's Photon OS Linux container within your virtualization
environment.

That is it on how to get PowerShell Core 6 in this distro for VMware vSphere
and Photon Linux folks.

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
- [GitHub Repo - VMware Photon - Wiki](https://github.com/vmware/photon/wiki)
- [GitHub Pages - VMware Photon - Documentation](https://vmware.github.io/photon/assets/files/html/1.0-2.0/index.html)

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
