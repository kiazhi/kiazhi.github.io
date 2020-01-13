---
layout: single
title: "Needing PowerShell in Parrot Linux container"
excerpt: "Introducing PowerShell in Parrot (Parrot Security, ParrotOS, Parrot
GNU/Linux) container using Dockerfile and DockerCLI with Docker for security
enthusiasts."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-10-24 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - Container
  - Linux
  - Parrot
  - PowerShell
  - "Ryen Tang"
---

With my previous blog post about
[Getting PowerShell in Kali Linux container](https://kiazhi.github.io/blog/powershell/Getting-PowerShell-in-Kali-Linux-container/),
I will be demonstrating on how you can install PowerShell 6 in
[Parrot](https://parrotsec.org/) (ParrotSec, Parrot GNU/Linux) distro container
today.

In this demonstration, I will not be providing a break down of the processes in
the `Dockerfile` with explaination on each command execution except including
comments in the `Dockerfile` to indicate what it is doing.

> For container newbies:
>
> I will highly recommends you to read my previous blog post
> [here](https://kiazhi.github.io/blog/powershell/Getting-PowerShell-in-Kali-Linux-container/)
> to get a good understanding of each break down, since those steps will be some
> how similar.

## Introducing PowerShell to Parrot container

In this section, I will provide a `Dockerfile` text document file that contains
all the commands to build a Parrot container and install PowerShell to the
Parrot Linux distro when you use DockerCLI `docker build` command.

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

Firstly, create a new `Dockerfile` text document file.
Next, copy the code below and paste it into that `Dockerfile` text document
file.
Remember to save the `Dockerfile` file and exit from your editor.

```dockerfile
# Pull official Parrot container image
FROM parrotsec/parrot-core:latest AS build

# Define Args and Env needed to create links
ENV PS_INSTALL_FOLDER=/opt/microsoft/powershell/6 \
    # Define ENVs for Localization/Globalization
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

# Download the PowerShell package for Debian9
ADD https://github.com/PowerShell/PowerShell/releases/download/v6.1.0/powershell_6.1.0-1.debian.9_amd64.deb \
    /tmp/powershell_6.1.0-1.debian.9_amd64.deb

# Download the libicu57 Debian package and save it
ADD http://ftp.us.debian.org/debian/pool/main/i/icu/libicu57_57.1-9_amd64.deb \
    /tmp/libicu57_57.1-9_amd64.deb

# Installation and configuration
RUN \
    # Update package list
    apt-get update \
    # Install dependencies
    && apt-get install -y \
      # Required package for help in powershell
        less \
      # Required package to setup the locale
        locales \
    # Enable en_US.UTF-8 locale
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    # Generate locale
    && locale-gen && update-locale \
    # install required libicu57 package
    && dpkg -i /tmp/libicu57_57.1-9_amd64.deb \
    # remove libicu57 package
    && rm -f /tmp/libicu57_57.1-9_amd64.deb \
    # Install powershell package
    && apt-get install -y /tmp/powershell_6.1.0-1.debian.9_amd64.deb \
    # Remove powershell package
    && rm -f /tmp/powershell_6.1.0-1.debian.9_amd64.deb \
    # Upgrade distro
    && apt-get dist-upgrade -y \
    # Clean downloaded packages
    && apt-get clean \
    # Remove package list
    && rm -rf /var/lib/apt/lists/*

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
Parrot container from the instructions in the `Dockerfile` text document file.

```sh
# Change to your working directory
cd \The\directory\where\your\Dockerfile\is\saved

# Build the container with the Dockerfile
docker build -t parrot .
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
command to fire it up and have a play with it in PowerShell.

```sh
docker run --rm --interactive parrot
```

If you requires to run the container in `bash`, you can do it on the same
container with the command below.

```sh
docker run --rm --interactive --tty parrot bin/bash
```

And if you need to go back into PowerShell from the `bash` in the container,
type `pwsh` to switch into PowerShell.

There you go for those parrot folks out there.

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
- [Parrot Docs - What is Parrot?](https://parrotsec.org/docs/01.Introduction/01.what-is-parrot/)

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
