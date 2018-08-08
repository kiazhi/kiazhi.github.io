---
layout: single
title: "Welcome to Jekyll on GitHub Pages"
excerpt: "My first attempt on getting a static blog site to blog in
Markdown/HTML language with my trusty keyboard and follow this Getting Started with
Jekyll on GitHub Pages."
author: Ryen Tang
date: 2018-07-01 00:00:00 +1200
toc: true
categories: 
  - Blog
tags:
  - Blog
  - Jekyll
  - GitHub
  - Ruby
  - "Ryen Tang"
---

Recently, I had been thinking about migrating my blog from
[ryentang.wordpress.com](https://ryentang.wordpress.com) to
[kiazhi.github.io](https://kiazhi.github.io). There are positive and negative
thoughts about it. Sure enough, I have mixed feelings on this dramatic move but
I still want to give it a try.

## Getting Started with Jekyll on GitHub

In this blog post, I will be documenting the basic steps to get Jekyll on GitHub
using command line from macOS. If you are a Windows person, the documentation
in this blog post will still be of use because it provides an understanding
of the flow and what needs to be done.

Why am I documenting this in a blog post? Because this is my first attempt with
Jekyll on GitHub and after that it will be history and I am taking a note of
this to help myself in remembering the process. :)

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

- [GitHub User Account](https://github.com/join)
- [Git](https://git-scm.com)
- [Ruby](https://ruby-lang.org)
    - [Bundler](https://bundler.io)
    - [Jekyll](https://jekyllrb.com/)

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

### Create a GitHub Repository

Create a [GitHub.io](https://github.io) repository using the command line
below.

> Note:
In order to make this easier for everyone to understand, I have chosen to use
`$GITHUB_USERNAME` and `$GITHUB_PASSWORD` as variables. You will need to
replace these `$GITHUB_USERNAME` and `$GITHUB_PASSWORD` variables with your
real GitHub username and password.

```sh
curl -u '$GITHUB_USERNAME:$GITHUB_PASSWORD' https://api.github.com/user/repos -d '{"name":"$GITHUB_USERNAME.github.io"}'
```

If you have setup your GitHub account with two-factor authentication, you will
need [create an access token](https://developer.github.com/v3/oauth_authorizations/#create-a-new-authorization)
to authenticate with [GitHub REST API](https://developer.github.com/v3/) and
use the command line in the example below.

> Note:
In order to make this easier for everyone to understand, I have chosen to use
`$GITHUB_ACCESS_TOKEN` as a variable. You will need to replace the
`$GITHUB_ACCESS_TOKEN` variable with an actual token string from your GitHub
User Settings.

```sh
curl -u '$GITHUB_USERNAME:$GITHUB_ACCESS_TOKEN' https://api.github.com/user/repos -d '{"name":"$GITHUB_USERNAME.github.io"}'
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

### Clone GitHub remote repository to local file system

Once you have created the repository, you will need to find a local file system
location to download the new empty repository to work with locally before
pushing content back to GitHub repository using Git for publishing the site
with static web content.

In this example, I will demostrate this using `git clone` command below.

> Note:
If you have followed this from the start, you will know that `$GITHUB_USERNAME`
is a variable and you need to replace this with your real GitHub username.

```sh
git clone https://github.com/$GITHUB_USERNAME/$GITHUB_USERNAME.github.io.git
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

### Install Bundler and Jekyll

Before we use `gem install` command on your terminal, make sure you have Ruby
installed. To validate Ruby is installed, execute the shell command `ruby -v`
in your terminal. If the command returns Ruby version in your terminal, proceed
to install Bundler and Jekyll. If the command did not returns Ruby version, you
can refer to the documentation on 
[Installing Ruby](https://www.ruby-lang.org/en/documentation/installation/).

```sh
gem install bundler jekyll
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

### Create your new Jekyll site

Change your current working directory to the cloned repository directory path.

```sh
cd $GITHUB_USERNAME.github.io
```

Use `jekyll new` to create your new Jekyll site in your current cloned
repository directory path in your local file system and see it populate the
original empty folder.

```sh
jekyll new .
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

### Generate static site and preview site locally

Next, we can generates the static site and preview the site locally use loopback
address. Remember that this command must only be executed to generate and
preview the site from the local file system GitHub
`./$GITHUB_USERNAME.github.io` repository directory where the Jekyll content is
located.

```sh
bundle exec jekyll serve
```

You can preview the site by browsing to `http://127.0.0.1:4000/` loopback
address on port 4000. Use `CTRL+C` on the terminal to stop Jekyll from
serving the site and returns back to shell.

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

### Publishing static site to GitHub.io

Finally, when you are satisfied with the site content and layout preview, use
`git` to add all files from local repository, commit them and push to your
GitHub repository.

```sh
git add --all
git commit -m 'My first Jekyll site commit'
git push -u origin master
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

Finally, you can view the published site on the internet by browsing to the url.

> Note:
Remember to replace `$GITHUB_USERNAME` variable with your real GitHub username
to browse your published online site.

On macOS terminal:

```sh
open https://$GITHUB_USERNAME.github.io/
```

Or launch any of your favourite browser and manually input the url.

It is simple to get a blog site up and running instantly without meddling with 
database requirements and etc. The disappointment factor is probably site
management capability and features that are offered by big service providers
(eg. Blogger, Wordpress and so on.).

I suppose the only reason that attracts me using Jekyll on GitHub Pages would be 
the ease of use with markdown language and the freedom with
[GitHub's terms of service](https://help.github.com/articles/github-terms-of-service/)
for
[GitHub Pages](https://help.github.com/articles/github-terms-of-service/#i-additional-terms-for-github-pages).

Well, this is the first post and I still have to take some time to fine tune
this site with the use of Ruby, CSS, HTML, Markdown, Git skillset to make this
site better and easier for me to manage. That also means I will have to hack
more on this and provide myself this new challenge. Hack-on...

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

# Related Books

<div id="amzn-assoc-ad-f3a340a5-ce4d-4b4c-b409-c4c202ba7ffe"></div><script async src="//z-na.amazon-adsystem.com/widgets/onejs?MarketPlace=US&adInstanceId=f3a340a5-ce4d-4b4c-b409-c4c202ba7ffe"></script>

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>