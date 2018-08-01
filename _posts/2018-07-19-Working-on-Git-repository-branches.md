---
layout: single
title: "Working with Git repository branches"
excerpt: "Branches growing everywhere? Sharing my digital horticultural
experience on how you can grow and prune those branches in a Git
repository."
author: Ryen Tang
header:
  image: ./assets/images/banners/Git-1280x200.png
date: 2018-07-19 00:00:00 +1200
toc: true
categories: 
  - Blog
  - Git
tags:
  - Blog
  - Git
  - "Ryen Tang"
---

If you have followed my previous blog
[post](https://kiazhi.github.io/blog/git/Working-with-Git-repository-for-the-first-time/)
on creating your first Git
repository, you may have observe that you are working on the master branch of
that Git repository.

In this blog post, I will be discussing about working on Git repository
branches and how you can work on a different branch within a Git repository. 

Why is it important to know about managing Git repository branches? Most
repositories out there has multiple branches for a special reason.

In most cases, developers wanted to make some major changes but still wants to
keep the current state available.

But in some cases, developers create other branches to cherry-pick those
specific commits for the next stable release.

Therefore, it is important to know how to manage those Git repository branches
without accidentally causing damage to the master branch.

> Note: Master branch is the default name of the first branch when a Git
repository is created.

> Note: A master branch of a Git repository has no special attribute and you
can delete a master branch just like any other branches.

## Viewing the available branches in a repository

Firstly, let us view the available branches from a local Git repository using
`git branch` with `-l` parameter. Assuming you are working on a new Git
repository, you should be able to find the output stating master branch. 

```sh
git branch -l
```

If you have clone a Git repository, you can use the `git branch` with `-r`
parameter to view all remote branches.

```sh
git branch -r
```

To get a bird's eye view of all the branches in local or remote Git repository,
use the `git branch` with `-a` parameter.

```sh
git branch -a
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Creating a new branch in a repository on local file system

Using the `git checkout` with `-b` parameter specified with a name of the new
branch, will create a new branch in a repository on the local file system.

> Note: This example will create a new branch that is a clone of your current
working branch on local file system.

> Note: The new branch will not exist in the project/repository remote
source until the branch is pushed to the remote Git repository source.

> Note: In order to make this easier for everyone to understand, I have chosen
to use `$NEW_BRANCH_NAME` or `$BRANCH_NAME` as a variable. You will need to
replace those variables with your real project/repository branch name.

```sh
git checkout -b $NEW_BRANCH_NAME
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Validating your current working branch in local repository

Using `git branch` does not just displays the available branches in a Git
repository, it also highlight your current working branch with an `*` in front
of the branch name.

```sh
git branch
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Switching between branches in local repository

Now that you have 2 branches in the Git repository, try switch back to the
`master` branch using `git checkout` and specify `master` branch name.

```sh
git checkout $BRANCH_NAME
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Pushing your local new branch to remote repository

Now, if you check your Git repository on your remote service provider or
self-hosting on-premise server, you probably cannot find the `$NEW_BRANCH_NAME`
branch and wondering on do you get your new branch from local Git repository to
the remote Git repository.

Use `git push` with `-u` parameter to specify the upstream location to the
`origin` where your remote repository is located and include the new branch
name to push your new branch from local file system to remote service provider
or self-hosting on-premise server.

```sh
git push -u origin $NEW_BRANCH_NAME
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Pushing changes between two branches

In this exercise, assuming that someone had made changes to `master` branch and
you would like to obtain those new changes from `master` branch in your new
branch before you start going ahead working in your new branch.

For demonstration purposes, let us make some changes to the local `master`
branch and commit those changes to the remote `master` branch in your Git
repository.

```sh
git checkout master
echo "# Changes" > Changes.md
echo "My Changes on Master Branch" >> Changes.md
git add Changes.md
git commit -m "Changes made to master branch"
git push
```

Using `git push` with `origin` that refers to the remote Git repository
location, you will specify `master` branch as remote source branch separated
by the `:` with remote destination branch name. Once `git push` pushes the
change from remote `master` branch to remote new branch has completed, you will
have to use `git pull` to pull the change made on remote new branch to your
local new branch.

> Note: This will not work if you already made changes to your remote new
branch before this recent change made to the remote `master` branch. That is
because your changes made to the remote new branch is already ahead of the
`master` branch.

```sh
git push origin master:$BRANCH_NAME
git pull
```

Alternatively, if your new branch is ahead of master branch, you could force
the push from remote `master` branch to remote new branch using the `-f`
parameter.

But I will highly recommend not to do so unless you are prepared to lose those
changes on the remote new branch that was previously made.

> Note: Forcing a push between branches when the tip of your current branch is
behind its remote counterpart will discard changes previously made on the
remote destination branch.

```sh
git push -f origin master:$BRANCH_NAME
git pull
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Deleting the branch in local repository

In order to delete the branch that we created from the beginning, use
`git branch` with `-d` parameter followed by the name of the branch you wants
to delete the branch from local Git repository.

> Note: This will not work if you are currently working on that branch. You
will need to checkout/switch to the master branch or any other branches in
order to delete that branch from the local file system.

> Note: This branch deletion only occur to the Git repository on local file
system and will not affect the remote Git repository.

```sh
git branch -d $BRANCH_NAME
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Deleting the branch in remote repository

For deleting the branch from the remote Git repository, you will have to use
`git push` with `origin` that refers to the remote Git repository location,
specify nothing or null as the source branch separated by the `:` with remote
destination branch name that you would like to delete.

In short, you are sending an instruction to the remote Git repository to push
nothing to the particular branch name. Since nothing is pushed to the
particular branch name, the particular branch has nothing and get deleted or
disappeared. I suppose the easiest way of understanding this is like
Nothing went In, Nothing came Out (NINO) and therefore it is gone.

> Note: This branch deletion only occur on remote Git repository and will not
affect the Git repository on local file system.

```sh
git push origin :$BRANCH_NAME
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Conclusion

That all, folks. These are the basic on how to manage branches in Git
repository and hope you gain some knowledge on how to grow or prune those
branches.

If you find that this information is useful, feel free to bookmark this or
share it with your colleague.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>
