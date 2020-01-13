---
layout: single
title: "Bringing PowerShell to Clear Linux container"
excerpt: "Finding a way to install PowerShell on Intel's Clear Linux container?
Learn how to install PowerShell into Clear Linux with multi-stage builds
Dockerfile."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-11-21 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - Container
  - Linux
  - "Clear Linux"
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

I will be demonstrating on how you can install PowerShell 6 in
[Clear Linux](https://clearlinux.org/) distro container.

Like my previous blog post, I will not be providing a break down of the steps
in the `Dockerfile` with explainations except including comments in the
`Dockerfile` to indicate what it is doing.

## PowerShell for Clear Linux container

In this section, I will provide a `Dockerfile` text document file that contains
all the commands to build a Clear Linux container and install PowerShell to
the Clear Linux distro when you use DockerCLI `docker build` command.

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
# Pull Clear Linux container image
FROM clearlinux:base AS stage

# Define Args and Env needed to create links
ENV PS_INSTALL_FOLDER=/opt/microsoft/powershell/6 \
    # Define ENVs for Localization/Globalization
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

# Download the PowerShell binary package
ADD https://github.com/PowerShell/PowerShell/releases/download/v6.1.0/powershell-6.1.0-linux-x64.tar.gz \
    /tmp/powershell-linux.tar.gz

# Download the less binary package
ADD https://ftp.gnu.org/gnu/less/less-530.tar.gz \
    /tmp/less.tar.gz

# Installation and configuration
RUN \
    # install dependencies
    swupd bundle-add \
      # required libstdc++.so.6
      # required bundle to make less executable
      os-core-dev \
    # create less folder
    && mkdir -p /tmp/less \
    # uncompress GNU/Linux less tar file
    && tar zxf /tmp/less.tar.gz -C /tmp/less --strip-components 1 \
    # make less executable
    && ./tmp/less/configure \
    # create powershell folder
    && mkdir -p ${PS_INSTALL_FOLDER} \
    # uncompress powershell linux tar file
    && tar zxf /tmp/powershell-linux.tar.gz -C ${PS_INSTALL_FOLDER}

# Start a new stage so we lose all the tar.gz layers from the final image
FROM clearlinux:base as build

# Copy only the files we need from the previous stage
COPY --from=stage ["/usr/bin/less", "/usr/bin/less"]

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
    DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER=0

RUN \
    # install dependencies
    swupd bundle-add \
      # required package for International Components for Unicode
      runtime-libs-boost \
    # Create the pwsh symbolic link that points to powershell
    && ln -s ${PS_INSTALL_FOLDER}/pwsh /usr/bin/pwsh \
    # upgrade distro
    && swupd update \
    # clean downloaded packages
    && swupd clean --all

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
Clear Linux container from the instructions in the `Dockerfile` text
document file.

```sh
# Change to your working directory
cd \The\directory\where\your\Dockerfile\is\saved

# Build the container with the Dockerfile
docker build -t clearlinux .
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
docker run --rm --interactive clearlinux
```

And if you need to go back into Bash from the `pwsh` in the container,
type `bin/bash` to switch into Bash.

But if you requires to run the container in `bash`, you can do it on the same
container with the command below.

```sh
docker run --rm --interactive --tty clearlinux bin/bash
```

And if you need to go back into PowerShell from the `bash` in the container,
type `pwsh` to switch into PowerShell.

That is it on how to get PowerShell Core 6 in this distro for Intel's Clear
Linux folks.

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
- [Clear Linux Project - Documentation](https://clearlinux.org/documentation/clear-linux)

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
