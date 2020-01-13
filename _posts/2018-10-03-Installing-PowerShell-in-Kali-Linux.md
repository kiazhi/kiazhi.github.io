---
layout: single
title: "Installing PowerShell in Kali Linux"
excerpt: "A guide on installing PowerShell in Kali Linux for Security
enthusiasts and how you simplify the installation using Microsoft public
package repository."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-10-03 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - Linux
  - Kali
  - PowerShell
  - "Ryen Tang"
---

After ploughing through the internet and found the the following
[Microsoft Docs - Installing PowerShell Core on Linux](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-linux#Kali)
documentation for Kali, I discovered the documented steps may have been
obselete and requires updating since there are a few people commenting the
installation for [Kali](https://www.kali.org/) just does not work with an
issue [#2901](https://github.com/PowerShell/PowerShell-Docs/issues/2901) raised
in [PowerShell/PowerShell-Docs](https://github.com/PowerShell/PowerShell-Docs)
public repository in [GitHub](https://github.com/).

**Original steps from documentation below:**
```sh
# Download & Install prerequisites
sudo apt-get install libunwind8 libicu55
wget http://security.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u6_amd64.deb
sudo dpkg -i libssl1.0.0_1.0.1t-1+deb8u6_amd64.deb

# Install PowerShell
sudo dpkg -i powershell_6.1.0-1.ubuntu.16.04_amd64.deb

# Start PowerShell
pwsh
```

Many may have been led astray on how to install PowerShell in Kali with the
steps above and I am going to attempt to correct it today until next version
is out.

## Installing PowerShell in Kali

In this section, I will walkthrough on an up to date steps on how you can
install PowerShell in Kali from the time when this blog post is published.

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

### Download & Install PowerShell dependencies

Firstly, you will have to install the `Libicu57` (International Components for
Unicode) package in order to install PowerShell to Kali Linux because
PowerShell 6.1 has a dependency with that package that contains a library for
Unicode support to interpret a whole heaps of stuffs.

To understand more about International Components for Unicode, you can read
about it
[here](https://en.wikipedia.org/wiki/International_Components_for_Unicode).

```sh
# Download Libicu57 (International Components for Unicode)
#  package from Debian source
wget http://ftp.us.debian.org/debian/pool/main/i/icu/libicu57_57.1-9_amd64.deb

# Use Debian package manager to install Libicu57 package
dpkg -i libicu57_57.1-9_amd64.deb
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

### Download & Install prerequisites

Next, you need to install the other supporting packages for setting up Advanced
Package Tool (APT) in order to add external public package repository and
ensuring you can obtain the package.

```sh
# Get Advanced Package Tool (APT) to update available
#  package list
apt-get update

# Use Advanced Package Tool (APT) to install the following
#  packages:
#   - curl
#   - gnupg
#   - apt-transport-https
apt-get install -y curl gnupg apt-transport-https
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

### Add Microsoft public repository key to APT as trusted repository

Once the required prerequisites are met, you need to add the Microsoft public
repository key to Advanced Package Tool (APT) to configure the repository as a
trusted source.

```sh
# Use curl to get the key and add the key to APT
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
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

### Add Microsoft package repository to the source list

With the Microsoft public repository key added to Advanced Package Tool (APT),
you will have to add the Microsoft public package repository configuration to
the source list so that APT will know how to connect to it using the key to
obtain a list of available packages.

```sh
# Write the package repository source to a powershell
#  source list file
echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-stretch-prod stretch main" | tee /etc/apt/sources.list.d/powershell.list
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

### Install the PowerShell package

Since you have added a new package repository to the source list, you will have
to use `apt-get update` to retrieve a list of available packages from that new
package repository to be available for installation.

Once the Advanced Package Tool (APT) has updated the available package list,
you can use `apt-get install` to install the PowerShell package.

```sh
# Get Advanced Package Tool (APT) to update available
#  package list
apt-get update

# Use Advanced Package Tool (APT) to install PowerShell
apt-get install -y powershell
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

### Switch into PowerShell

After the PowerShell package installation has completed, you can use `pwsh` to
switch to PowerShell from Bash.

```sh
# Start PowerShell
pwsh
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

### Uninstalling PowerShell

To remove PowerShell from Kali Linux, you can use `apt-get remove` to uninstall
the PowerShell package from Bash.

```sh
# Uninstall PowerShell package
apt-get remove -y powershell
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

There you have it on how you can obtain PowerShell in Kali.

While you have read this blog post, I have updated the documentation for
Microsoft and submitted a pull request
[#2973](https://github.com/PowerShell/PowerShell-Docs/pull/2973) to ensure
everyone that are googling out there can get the same method on installing
PowerShell in Kali.

Until the pull request has been approved to merge, you can follow the steps in
this blog post.

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
