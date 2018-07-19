---
layout: single
title: "Working with Git repository for the first time"
excerpt: "Are you frustrated with version control? Explore using Git with this
guide or cheat sheet on working with Git repository for the first time."
author: Ryen Tang
header:
  image: ./assets/images/banners/Git-1280x200.png
date: 2018-07-18 00:00:00 +1200
toc: true
categories: 
  - Blog
  - Git
tags:
  - Blog
  - Git
  - "Ryen Tang"
---

In this blog post, I will be sharing basic Git commands that I frequently
use and hope that it is an useful knowledge for anyone who just started working
with `git` for their first time. This is my own version of summarized cheat
sheet to help anyone on common tasks. If you have never used `git` before, let
me share this with you to get you onboard this cool band wagon.

**What is Git?** Git is a distributed version control system created by Linus
Torvalds and developed or maintained by the community. To find out more about
Git, please refer to [here](https://en.wikipedia.org/wiki/Git).

## Getting Started with Basic Git Commands

To get started, I assumes you have a project or repository from any of the
public service provider (eg.
[GitHub](https://github.com/),
[GitLab](https://gitlab.com/),
[BitBucket](https://bitbucket.org/)
or etc) or self-hosting on-premise (eg.
[Gogs](https://gogs.io/),
[Trac](https://trac.edgewall.org/),
[Phabricator](https://phacility.com/phabricator/)
or etc) solution. Of course, the list of service providers and self-hosting
on-premise solutions that are available in the market goes on.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Pre-requisite requirements

- [Git](https://git-scm.com/downloads)
  - [Linux/Unix](https://git-scm.com/download/linux)
  - [macOS](https://git-scm.com/download/mac)
  - [Windows](https://git-scm.com/download/win)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Initialize a project or repository on local file system

In this example, I will demonstrate on how you can initialize a new repository
on local file system and synchronise with your remote repository on the server
or service provider.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Create a folder for your repository

Firstly, we will create a folder `C:\Repositories` that will contain all your
individual repository and a subfolder `MyFirstRepository` to work with. Next,
we will change our current working location to that subfolder location.

```sh
mkdir \Repositories\MyFirstRepository
cd \Repositories\MyFirstRepository
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Initialize the folder as a Git repository

Now, let us initialize this subfolder as a Git repository

```sh
git init
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Create file in the repository folder

For demonstration, we will create a `HelloWorld.md` markdown file in the
subfolder but you can skip this if you already have files that you wanted to be
in the repository.

```sh
echo "# Hello World" > HelloWorld.md
echo "My First Repository" >> HelloWorld.md
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Add and stage files in the repository

Use `git add` to add and stage all files in the current repository on local
file system.

```sh
git add .
```

Or you could add and stage specific files by specifying each file seperated by
blank space.

```sh
git add HelloWorld.md GoodbyeWorld.md 
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Commit staged files in the repository

Use `git commit` to commits the staged files and include `-m` to add a message
to describe what you are preparing to commit changes to the repository on local
file system

```sh
git commit -m "My first commit to repository"
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Add a remote repository location

Use `git remote add` to add a remote repository URL that you obtain from a
service provider or self-hosting on-premise server to your repository on local
file system where your repository will synchronise with as distributed version
control primary source. Once you have added the remote repository URL, use
`git remote -v` to validate the remote repository URL is configured.

> Note: In order to make this easier for everyone to understand, I have chosen
to use `$REPOSITORY_URL` as a variable. You will need to replace the
`$REPOSITORY_URL` variable with your real project/repository web URL.

> Note: Below are some of common service providers `$REPOSITORY_URL` examples.
> - `https://github.com`/`YourUsername`/`YourRepositoryName`.git
> - `https://gitlab.com`/`YourUsername`/`YourProjectName`.git
> - `https://bitbucket.org`/`YourUsername`/`YourRepositoryName`.git

```sh
git remote add origin $REPOSITORY_URL
git remote -v
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Push the committed files to remote repository

Finally, use `git push` to push the committed changes from your repository on
local file system to the repository on remote service provider or self-hosting
on-premise server.

```sh
git push origin master
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Check the repository status

You can use `git status` to check the status between your repository on local
file system against the repository on remote service provider or self-hosting
on-premise server.

```sh
git status
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Cloning a project or repository to local file system

If you are working on an existing project or repository that is already on
remote service provider or self-host on-premise server, you will just have to
clone the project or repository from the remote source to your local file
system.

```sh
git clone $REPOSITORY_URL
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Viewing the repository history of changes

Once you have your Git repository ready on your local file system, you can use
`git log` to view the history of changes

```sh
git log
```

## Conclusion

It is just that simple to start having a Git repository to work with your files
and track those changes. Remember the key benefit of having Git repository is
that Git tracks content not files. Having a remote source from a remote service
provider or self-host on-premise server to store those Git repositories allows
others to work collaboratively on the repository with Git managing and tracking
those changes. There you have it, start gitting.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## References

- [Git Repository](https://github.com/git/git/)
- [Git Documentation](https://git-scm.com/docs)
- [Pro Git Book](https://git-scm.com/book/en/v2)
- [GitHub - Git Cheat Sheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>
