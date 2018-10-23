---
layout: single
title: "Getting PowerShell in Kali Linux container"
excerpt: "A walkthrough guide on getting PowerShell Core 6 in Kali Linux
container with Dockerfile and DockerCLI for Docker containerization
enthusiasts."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-10-16 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - Container
  - Linux
  - Kali
  - PowerShell
  - "Ryen Tang"
---

Continuing with my previous blog post about
[Installing PowerShell in Kali Linux](https://kiazhi.github.io/blog/powershell/Installing-PowerShell-in-Kali-Linux/),
I will share on how you can install PowerShell in Kali Linux container with
Dockerfile.

## Getting PowerShell in Kali Linux container

In this section, I will provide a walkthrough on scripting the installation of
PowerShell to Kali Linux container in a Dockefile that will be used by the
DockerCLI `docker build` command.

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

### Pulling Kali Linux container image

Firstly, you will have to launch Visual Studio Code or any text editor to
create a `Dockerfile` with the first line instructing `docker build` to pull an
official Kali Linux container image from the official Kali Linux container
register repository namespace as the base image layer.

```dockerfile
# Pull official Kali Linux container image
FROM kalilinux/kali-linux-docker:kali-rolling AS build
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

### Prepare the Kali Linux environment variables

Next, we will add the following environment variables using the `ENV`
Dockerfile command:

- `$PS_INSTALL_FOLDER`
- `$DOTNET_SYSTEM_GLOBALIZATION_INVARIANT`
- `$LC_ALL`
- `$LANG`

```dockerfile
# Define Args and Env needed to create links
ENV PS_INSTALL_FOLDER=/opt/microsoft/powershell/6

# Define ENVs for Localization/Globalization
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false
ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8
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

### Download the Kali repository certificate package

In order to download the Kali package to renew Kali package repository
certificate, we will add Kali package URI and output the downloaded file to the
`/tmp` folder using the `ADD` Dockerfile command.

```dockerfile
# Download the Kali repository package
ADD https://http.kali.org/kali/pool/main/k/kali-archive-keyring/kali-archive-keyring_2018.1_all.deb \
    /tmp/kali-archive-keyring_2018.1_all.deb
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

### Download the International Components for Unicode package

To install PowerShell Core 6.x, we will need to download the required
International Components for Unicode package for installation. We will add the
`libicu57` package URI from Debian package repository and output the downloaded
file to the `/tmp` folder using the `ADD` Dockerfile command.

```dockerfile
# Download the libicu57 Debian package
ADD http://ftp.us.debian.org/debian/pool/main/i/icu/libicu57_57.1-9_amd64.deb \
    /tmp/libicu57_57.1-9_amd64.deb
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

### Download the PowerShell Core 6.1 package

Finally, we will need the PowerShell Core 6.1 package from the
`PowerShell\PowerShell` repository from GitHub. We will add the
`powershell_6.1.0-1.debian.9_amd64.deb` package for Debian9 and output the
downloaded file to the `/tmp` folder using the `ADD` Dockerfile command.

```dockerfile
# Download the PowerShell package for Debian9
ADD https://github.com/PowerShell/PowerShell/releases/download/v6.1.0/powershell_6.1.0-1.debian.9_amd64.deb \
    /tmp/powershell_6.1.0-1.debian.9_amd64.deb
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

### Install the Kali repository certificate package

Before we can use `apt-get update` to update the available packages from the
Kali package repository, we will need to install the downloaded
`/tmp/kali-archive-keyring_2018.1_all.deb` package file to renew the
certificate for authenticating with Kali package repository.

We will add the `RUN` Dockerfile command to execute the `apt-get install`
command to install the downloaded package and initiate `rm -f` to remove the
downloaded package file after installation.

```dockerfile
# Renew Kali repository expired certificate in the
# container image
RUN apt-get install /tmp/kali-archive-keyring_2018.1_all.deb \
    # Remove the downloaded package file
    && rm -f /tmp/kali-archive-keyring_2018.1_all.deb
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

### Update the available package list

Once the `kali-archive-keyring_2018.1_all.deb` package has been installed, we
will add the `RUN` Dockerfile command to initiate `apt-get update` command to
update the available packages from Kali package repository.

```dockerfile
# Update package list
RUN apt-get update
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

### Install the International Components for Unicode package

After the update has completed, we will add the `RUN` Dockerfile command to
execute `dpkg -i` Debian package management command to install the downloaded
`/tmp/libicu57_57.1-9_amd64.deb` package and initiate `rm -f` to remove the
downloaded package file after installation.

```dockerfile
# Install required libicu57
# (International Components for Unicode) package
RUN dpkg -i /tmp/libicu57_57.1-9_amd64.deb \
    # Remove the downloaded package file
    && rm -f /tmp/libicu57_57.1-9_amd64.deb
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

### Install the other PowerShell dependencies

Now, we will install other PowerShell dependencies such as:

- `less` package for a terminal pager program on Unix-like systems used to view
  the contents of a text file one screen at a time for PowerShell's Help
- `locales` package to define language and country specific setting for your
  programs and shell session
- `ca-certificates` package to handle SSL certificates for PowerShell

To do that, we add the `RUN` Dockerfile command to execute the
`apt-get install -y` command for those packages using `RUN` Dockerfile command
to install those packages in their own independent stage during the build.

```dockerfile
# Required less package for help in powershell
RUN apt-get install -y less

# Required locales package to setup the locale
RUN apt-get install -y locales

# Required ca-certificates package for SSL
RUN apt-get install -y ca-certificates
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

### Configure Kali system localization

With `locales` package installed, we will add the `RUN` Dockerfile command to
`echo` an uncommented `"en_US.UTF-8 UTF-8"` string for American-English in the
`/etc/locale.gen` file, generate the locale using the `locale-gen` command, and
initate an update using `update-locale` command.

```dockerfile
# Enable specific locale
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    # Generate the specific locale
    && locale-gen \
    # Update the locale
    && update-locale
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

### Install the PowerShell Core 6.1

Then, we will add the `RUN` Dockerfile command to execute the
`apt-get install -y` command to install the
`/tmp/powershell_6.1.0-1.debian.9_amd64.deb` PowerShell package for Debian9 and
initiate `rm -f` to remove the downloaded package file after installation.

```dockerfile
# Install PowerShell package
RUN apt-get install -y /tmp/powershell_6.1.0-1.debian.9_amd64.deb \
    # Remove the downloaded package file
    && rm -f /tmp/powershell_6.1.0-1.debian.9_amd64.deb
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

### Update Kali distro

This is optional. If you requires Kali Linux distro to be up to date, you can
add the `RUN` Dockerfile command to initiate `apt-get dist-upgrade -y` command
to upgrade the distro.

After the distro upgrade has completes, you can clear the retrieved package
files in local repository using `apt-get clean` command, followed by removing
the packages list using the `rm -rf /var/lib/apt/lists/*` command.

```dockerfile
# Upgrade the Kali distro
RUN apt-get dist-upgrade -y \
    # Clear retrieved package files in local repository
    && apt-get clean \
    # Clear the packages list download from Kali package repository
    && rm -rf /var/lib/apt/lists/*
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

### Set the default shell to PowerShell

This is also optional. If you prefers Kali Linux to set PowerShell Core 6 as
the default shell environment for your container, you can add the `ENTRYPOINT`
Dockerfile command with `pwsh -c` and `CMD` Dockerfile command with `pwsh`.

```dockerfile
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

### Build the Kali container image

Finally, save your `Dockerfile` file with your Visual Studio Code or text
editor and use the DockerCLI with the command `docker build` and `--tag`
parameter to specify your container image name and tag in the folder location
where your Dockerfile is located.

```sh
# Navigate to your directory where you saved your Dockerfile file
# Build the container image from content in the Dockerfile file
docker build --tag powershell:kali-rolling .
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

After the container image build has completed, you can test the container image
using the DockerCLI commands like the example below:

- `docker` DockerCLI executable
- `run` Run a command in a new container
- `--rm` Automatically remove the container when it exits
- `--interactive` Keep STDIN open even if not attached
- `powershell:kali-rolling` Container Name followed `:` by Tag Name
- `$PSVersionTable` Show PowerShell Version Table

```sh
docker run --rm --interactive powershell:kali-rolling $PSVersionTable
```

If you have added the `ENTRYPOINT` in the Dockerfile for the build, the output
will be returned as below:

```text
Name                           Value
----                           -----
PSVersion                      6.1.0
PSEdition                      Core
GitCommitId                    6.1.0
OS                             Linux 4.9.125-linuxkit #1 SMP Fri Sep 7 08:20:28 UTC 2018
Platform                       Unix
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```

If you need to interactively work in the container, you can use the DockerCLI
commands with the example below.

```sh
docker run --rm --interactive powershell:kali-rolling
```

If you have added the `CMD` in the Dockerfile for the build, you will obtain
PowerShell as the default shell in the container interactive session.

You can switch between `pwsh`, `bash` or `sh` shells with the example below.

```sh
PowerShell 6.1.0
Copyright (c) Microsoft Corporation. All rights reserved.

https://aka.ms/pscore6-docs
Type 'help' to get help.

PS /> # Switch from pwsh to bash
PS /> /bin/bash

root@83a669d2bd41:/# # Exit from bash back to pwsh
root@83a669d2bd41:/# exit

PS /> # Switch from pwsh to system shell
PS /> /bin/sh

# # Exit from system shell to pwsh
# exit

PS />
```

There you have it on how you can obtain PowerShell in Kali Linux container.

> Note:
>
> If you followed the walkthrough above line by line, please kindly take
> note that it is not the most optimized way of building a container containing
> multiple `RUN` in Dockerfile because each `RUN` creates a new layer.
>
> The intent of this blog post structure containing multiple `RUN` is purely
> for educational purposes for people who are new to Docker and PowerShell on
> Kali Linux.
>
> For a more optimized container build, you can refer to the following
> Dockerfile below.

```dockerfile
# Pull official Kali Linux container image
FROM kalilinux/kali-linux-docker:kali-rolling AS build

# Define Args and Env needed to create links
ENV PS_INSTALL_FOLDER=/opt/microsoft/powershell/6 \
    # Define ENVs for Localization/Globalization
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

# Download the Kali repository package
ADD https://http.kali.org/kali/pool/main/k/kali-archive-keyring/kali-archive-keyring_2018.1_all.deb \
    /tmp/kali-archive-keyring_2018.1_all.deb

# Download the libicu57 Debian package
ADD http://ftp.us.debian.org/debian/pool/main/i/icu/libicu57_57.1-9_amd64.deb \
    /tmp/libicu57_57.1-9_amd64.deb

# Download the PowerShell package for Debian9
ADD https://github.com/PowerShell/PowerShell/releases/download/v6.1.0/powershell_6.1.0-1.debian.9_amd64.deb \
    /tmp/powershell_6.1.0-1.debian.9_amd64.deb

# Install and Configure
RUN \
    # Renew Kali repository expired certificate in the
    # container image
    apt-get install /tmp/kali-archive-keyring_2018.1_all.deb \
    # Remove the downloaded package file
    && rm -f /tmp/kali-archive-keyring_2018.1_all.deb \
    # Update package list
    && apt-get update \
    # Install required libicu57
    # (International Components for Unicode) package
    && dpkg -i /tmp/libicu57_57.1-9_amd64.deb \
    # Remove the downloaded package file
    && rm -f /tmp/libicu57_57.1-9_amd64.deb \
    # Install dependencies
    && apt-get install -y \
      # Required ca-certificates package for SSL
      ca-certificates \
      # Required less package for help in powershell
      less \
      # Required locales package to setup the locale
      locales \
    # Enable specific locale
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    # Generate the specific locale
    && locale-gen \
    # Update the locale
    && update-locale \
    # Install PowerShell package
    && apt-get install -y /tmp/powershell_6.1.0-1.debian.9_amd64.deb \
    # Remove the downloaded package file
    && rm -f /tmp/powershell_6.1.0-1.debian.9_amd64.deb \
    # Upgrade the Kali distro
    && apt-get dist-upgrade -y \
    # Clear retrieved package files in local repository
    && apt-get clean \
    # Clear the packages list download from Kali package repository
    && rm -rf /var/lib/apt/lists/*

# Configure entrypoint for the container
ENTRYPOINT ["pwsh", "-c"]

# Configure PowerShell as default shell for the container
CMD ["pwsh"]
```

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
- [Kali - Installing PowerShell on Kali Linux](https://www.kali.org/tutorials/installing-powershell-on-kali-linux/)

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