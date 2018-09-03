---
layout: single
title: "Working with Ruby and Jekyll on Windows for GitHub Pages"
excerpt: "A guide on working with Ruby and Jekyll on Windows 10 using Windows
Subsystem for Linux to generate static web before publishing to GitHub Pages."
author: Ryen Tang
header:
  image: ./assets/images/banners/Windows-1280x200.png
date: 2018-07-17 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Jekyll
  - GitHub
  - Ruby
  - Windows
  - "Windows Subsystem for Linux"
  - "Ryen Tang"
---

Previously, I started off with Jekyll using Ruby to generate this blog site on
macOS and I am stuck in trying to update this site with a Windows 10 machine
today.
Do you desperately wanted to deploy Jekyll using Ruby on Windows?
Honestly, I tried installing Ruby with Dev Kit on Windows 10, using gem to
install `bundler` and `jekyll`. But discovered it is not as easy as it seems
when I kept getting issues with some gems `http_parser`, `commonmarker` or etc.
If you google about Jekyll and Windows 10 issue, the list just goes on and on.

After so much frustration, there is a Windows solution that just get Ruby to
play nicely on Windows 10 and that is to use Windows Subsystem for Linux. Let's
start bashing on Windows 10.

## Getting Started with Jekyll on Windows 10 using Windows Subsystem for Linux

In this blog post, I will be documenting the basic steps to get Ruby for Jekyll
working on Windows 10 using Windows Subsystem for Linux (WSL) feature.

What is actually Windows Subsystem for Linux? In short, it is a Windows feature
on Windows 10 that allows developers to run Linux environment directly on
Windows without deploying a virtual machine. That means you get to use `Bash`
and many other Linux tools on Windows.

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

- Windows 10 Build [16215](https://docs.microsoft.com/en-us/windows/wsl/release-notes#build-16215)
or later

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

### Enable Windows Subsystem for Linux feature on Windows 10

To enable the Windows Subsystem for Linux Feature on Windows 10 and reboot the Windows 10 using PowerShell

> Note: A reboot of the Windows 10 operating system is required after enabling the Windows Subsystem for Linux feature

- Launch Windows PowerShell with elevated privileges
- Use the `Enable-WindowsOptionalFeature` PowerShell cmdlet to enable the feature

```powershell
Enable-WindowsOptionalFeature `
    -FeatureName Microsoft-Windows-Subsystem-Linux `
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

### Install Bash from Ubuntu distro

After the Windows 10 reboot, we can start installing `bash` on the Windows 10
operating system.

- Launch Command Prompt with elevated privileges
- Use `lxrun` command to install Bash from the Ubuntu distro image (default
distro)

```sh
lxrun /install /y
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

### Switching to Bash shell

Once the installation has completed, you can begin switching into Bash shell
runing from the Linux environment from the Command Prompt or PowerShell Console
on Windows 10.

```sh
bash
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

### Update the Bash shell image

Since the Windows Subsystem for Linux release with those packaged environment
images, those images may be out of date and we will need to manually perform an
update and upgrade for that Linux environment.

```sh
sudo apt-get update -y && sudo apt-get upgrade -y
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

### Install Ruby

Now that we have an up to date Linux environment, we can start installing Ruby
into that Linux environment using the Bash shell.

```sh
sudo apt-add-repository ppa:brightbox/ruby-ng
sudo apt-get update
sudo apt-get install ruby2.4 ruby2.4-dev build-essential dh-autoreconf
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

### Update Ruby Gems

With Ruby installed, let's update those Ruby gems to make sure we are all up to
date.

```sh
sudo gem update
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

### Install Jekyll with Bundler

With Bash installed using Windows Subsystem for Linux on Windows 10, Ruby
installed using Bash and Ruby Gems up to date in the Linux environment. We are
all set to start installing Jekyll using Bash on Windows 10 with the same
command that we used on macOS previously.

```sh
sudo gem install jekyll bundler
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

Finally, you can validate Jeykll installation status from Bash by validating
the Jekyll version. If nothing goes wrong, the Bash will returns you the
version number.

```sh
jekyll -v
```

To fully complete this demonstration, you should be able to use the
`jekyll new` command to generate a site and use `bundle exec jekyll serve` to
generate the static web site and view the site locally on
[http://127.0.0.1:4000/](http://127.0.0.1:4000/) loopback address on port 4000.

```sh
jekyll new my_blog
bundle exec jekyll serve
```

That is all, folks. It is just that simple on Bash with Windows Subsystem for
Linux on Windows 10 that ends my mickey mouse around with Ruby and Dev Kits on Windows.

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

- [Microsoft Docs: Windows Subsystem for Linux Documentation](https://docs.microsoft.com/en-us/windows/wsl/about)
- [Microsoft Docs: Windows 10 Installation Guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

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

<div id="amzn-assoc-ad-f3a340a5-ce4d-4b4c-b409-c4c202ba7ffe"></div><script async src="//z-na.amazon-adsystem.com/widgets/onejs?MarketPlace=US&adInstanceId=f3a340a5-ce4d-4b4c-b409-c4c202ba7ffe"></script>

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