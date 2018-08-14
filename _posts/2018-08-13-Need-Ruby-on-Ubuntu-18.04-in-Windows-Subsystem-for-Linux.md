---
layout: single
title: "Need Ruby on Ubuntu 18.04 in Windows Subsystem for Linux"
excerpt: "If you are having trouble installing Ruby 2.4 on Ubuntu 18.04 in Windows
Subsystem for Linux, this is what you need to do on Ubuntu 18.04"
author: Ryen Tang
header:
  image: ./assets/images/banners/Windows-1280x200.png
date: 2018-08-13 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Ruby
  - Ubuntu
  - Windows
  - "Windows Subsystem for Linux"
  - "Ryen Tang"
---

Recently, I decided to unregister my existing Ubuntu 16.04 on Windows Subsystem
for Linux (WSL) and start over again to try to upgrade it to Ubuntu 18.04 to
create this
[Getting Ubuntu distro environment on Windows for DevOps](https://kiazhi.github.io/blog/Getting-Ubuntu-distro-environment-on-Windows-for-DevOps/)
blog post.

After I have upgraded to Ubuntu 18.04, I tried to follow those steps again to
obtain Ruby 2.4 for working with Jeykll from my previous
[Working with Ruby and Jekyll on Windows for GitHub Pages](https://kiazhi.github.io/blog/Working-with-Jekyll-and-Ruby-on-Windows-for-GitHub-Pages/)
blog post and stumble on this Ruby installation issue.

## Problem with Ruby 2.4 installation on Ubuntu 18.04

In this section, I will show you what is the issue that I have encountered with
my Ubuntu 18.04 on Windows Subsystem for Linux (WSL).

To start off, I will display my Ubuntu version on WSL using the `lsb_release -a`
bash command.

```sh
# Display Ubuntu version
lsb_release -a
```

If you have upgraded your Ubuntu on WSL to 18.04 version, you should obtain the
following output below.

```text
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 18.04.1 LTS
Release:        18.04
Codename:       bionic
```

Next, I will follow the same command that I have used to install Ruby on Ubuntu
16.04 on WSL with the `apt-get install` bash command.

```sh
# Installing Ruby 2.4 on Ubuntu 18.04
sudo apt-get install ruby2.4 ruby2.4-dev build-essential dh-autoreconf
```

Once you executed the Ruby 2.4 package installation, you will obtain the
following installation error message output like below.

```text
Reading package lists... Done
Building dependency tree
Reading state information... Done
E: Unable to locate package ruby2.4
E: Couldn't find any package by glob 'ruby2.4'
E: Couldn't find any package by regex 'ruby2.4'
E: Unable to locate package ruby2.4-dev
E: Couldn't find any package by glob 'ruby2.4-dev'
E: Couldn't find any package by regex 'ruby2.4-dev'
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

## Remediating Ruby installation issue on Ubuntu 18.04

To remediate this Ruby installation issue on Ubuntu 18.04, you will need to
install Ruby 2.5 instead.

```sh
# Installing Ruby 2.5 on Ubuntu 18.04
sudo apt-get update
sudo apt-get install ruby2.5 ruby2.5-dev build-essential dh-autoreconf
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

What actually went wrong?

Well, there is nothing wrong with your Windows
Subsystem for Linux (WSL). The actual problem is because Ruby 2.4 package is
not available for Ubuntu 18.04 bionic and Ruby 2.5 is the recommended package
instead.

To check if the package you are looking for is available for your Ubuntu
version, visit the [Ubuntu Packages Search](https://packages.ubuntu.com/)
site and search the package name.

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

## Reference

- [Ubuntu Packages Search](https://packages.ubuntu.com/)
- [Ruby 2.5.0 Released](https://www.ruby-lang.org/en/news/2017/12/25/ruby-2-5-0-released/)
- [Ruby 2.5.1 Released](https://www.ruby-lang.org/en/news/2018/03/28/ruby-2-5-1-released/)
- [Windows Subsystem for Linux Documentation](https://docs.microsoft.com/en-us/windows/wsl/about)

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