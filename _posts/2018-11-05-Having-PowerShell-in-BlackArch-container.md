---
layout: single
title: "Having PowerShell in BlackArch container"
excerpt: "Learn how to have PowerShell as part of the arsenal of tools in
BlackArch Linux container for BlackArch or SecOps folks out there with this
walkthrough."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-11-05 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - Container
  - Linux
  - BlackArch
  - PowerShell
  - "Ryen Tang"
---

With my previous blog posts about installing PowerShell on the following Linux
distros:

- [Kali Linux](https://kiazhi.github.io/blog/powershell/Getting-PowerShell-in-Kali-Linux-container/)
- [Parrot Linux](https://kiazhi.github.io/blog/powershell/Needing-PowerShell-in-Parrot-Linux-container/)
- [Arch Linux](https://kiazhi.github.io/blog/powershell/Looking-for-PowerShell-in-Arch-Linux-container/)

I will be demonstrating on how you can install PowerShell 6 in
[BlackArch](https://blackarch.org/) Linux that is based on Arch Linux
(Arch GNU/Linux) distro container today.

Like my previous blog post, I will not be providing a break down of the steps
in the `Dockerfile` with explainations except including comments in the
`Dockerfile` to indicate what it is doing.

## PowerShell for BlackArch container

In this section, I will provide a `Dockerfile` text document file that contains
all the commands to build a BlackArch Linux container and install PowerShell to
the BlackArch Linux distro when you use DockerCLI `docker build` command.

> Note:
>
> Since BlackArch Linux derives from Arch Linux, I will have to build the
> BlackArch container using the Arch Linux container image.
>
> Although at the time of this blog post,
> [Arch Linux Wiki - Docker Images](https://wiki.archlinux.org/index.php/docker#Images)
> states that we should be using `docker pull archlinux/base` for Arch Linux
> container image. I have chosen to use `docker pull base/archlinux` from the
> [base/archlinux](https://hub.docker.com/r/base/archlinux/)
> in Docker Hub because it is more up to date and maintained by a community in
> [GitHub - ArchImg/ArchLinux](https://github.com/archimg/archlinux).

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
# Pull Arch Linux container image
FROM base/archlinux:latest AS build

# Define Args and Env needed to create links
ENV PS_INSTALL_FOLDER=/opt/microsoft/powershell/6 \
    # Define ENVs for Localization/Globalization
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

# Download BlackArch Linux strap shell script
ADD https://blackarch.org/strap.sh \
    /tmp/strap.sh

# Download the PowerShell binary package
ADD https://github.com/PowerShell/PowerShell/releases/download/v6.1.0/powershell-6.1.0-linux-x64.tar.gz \
    /tmp/powershell-linux.tar.gz

# Installation and configuration
RUN \
    # Enable en_US.UTF-8 locale
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    # Generate locale
    && locale-gen \
    # Set execute permission on BlackArch strap shell script
    && chmod +x /tmp/strap.sh \
    # Run BlackArch strap shell script
    && /tmp/strap.sh \
    # Remove BlackArch strap shell script
    && rm -f /tmp/strap.sh \
    # Update package database
    && pacman -Syy \
    # Install dependencies
    && pacman -S --noconfirm \
      # Required package for International Components for Unicode
      core/icu \
      # Required package for SSL
      openssl-1.0 \
    # Create powershell folder
    && mkdir -p ${PS_INSTALL_FOLDER} \
    # Uncompress powershell linux tar file
    && tar zxf /tmp/powershell-linux.tar.gz -C ${PS_INSTALL_FOLDER} \
    # Remove powershell linux tar file
    && rm -f /tmp/powershell-linux.tar.gz \
    # Create the pwsh symbolic link that points to powershell
    && ln -s ${PS_INSTALL_FOLDER}/pwsh /usr/bin/pwsh \
    # Upgrade distro
    && pacman -Syyu --noconfirm \
    # Clean downloaded packages
    && yes | pacman -Scc

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
BlackArch Linux container from the instructions in the `Dockerfile` text
document file.

```sh
# Change to your working directory
cd \The\directory\where\your\Dockerfile\is\saved

# Build the container with the Dockerfile
docker build -t blackarch .
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
docker run --rm --interactive blackarch
```

And if you need to go back into Bash from the `pwsh` in the container,
type `bin/bash` to switch into Bash.

But if you requires to run the container in `bash`, you can do it on the same
container with the command below.

```sh
docker run --rm --interactive --tty blackarch bin/bash
```

And if you need to go back into PowerShell from the `bash` in the container,
type `pwsh` to switch into PowerShell.

That is it on how to get PowerShell Core 6 in this distro for BlackArch or
SecOps folks.

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
- [BlackArch Linux Guide - Documentation](https://blackarch.org/guide.html)

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