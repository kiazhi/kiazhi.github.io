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

If you have followed my previous blog post on creating your first Git
repository, you may have observe that you are working on the master branch of
that Git repository.

In this blog post, I will be discussing about working on Git repository
branches and how you can work on a different branch within a Git repository. 

Why is it important to know about managing Git repository branches? Most
repositories out there has multiple branches for a special reason. In most
cases, developers wanted to make some major changes but still wants to keep the
current state available. In some case, developers create other branches to
cherry-pick those specific commits for the next stable release. So it is
important to know how to manage those Git repository branches without
accidentally causing damage to the master branch.

> Note: Master branch is the default name of the first branch when a Git
repository is created.

> Note: A master branch of a Git repository has no special attribute and you
can delete a master branch like just any other branches.

## Viewing the available branches in a repository

Firstly, let us view the available branches from a repository using
`git branch` command. Assuming you are working on a new Git repository, you
should be able to see a master branch output. 

```sh
git branch
```

## Creating a new branch in a repository on local file system

Using the `git checkout` with `-b` parameter specified with a name of the new
branch, will create a new branch in a repository on the local file system.

> Note: This example will create a new branch that is a clone of your current
working branch on local file system.

> Note: The new branch will not exist in the project/repository online
source until the branch is pushed to the online source.

> Note: In order to make this easier for everyone to understand, I have chosen
to use `$NEW_BRANCH_NAME` or `$BRANCH_NAME` as a variable. You will need to
replace those variables with your real project/repository branch name.

```sh
git checkout -b $NEW_BRANCH_NAME
```

## Validate your current working branch in local repository

Using `git branch` does not just displays the available branches in a Git
repository, it also highlight your current working branch with an `*` in front
of the branch name.

```sh
git branch
```

## Switch between branches in local repository

Now that you have 2 branches in the Git repository, try switch back to the
`master` branch using `git checkout` and specify `master` branch name.

```sh
git checkout $BRANCH_NAME
```

## Push your local new branch to remote repository

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

## Synchronise two branches

```sh
git checkout master
echo "# Changes" > Changes.md
echo "My Changes on Master Branch" >> Changes.md
git add Changes.md
git commit -m "changes made to master branch"
git push
```

```sh
git push origin master:$BRANCH_NAME
git pull
```


## Cloning a branch from a project or repository to local file system

In this example, you can clone an existing project or repository from the
online source to your local file system using the following command
below.

```sh
git clone -b $BRANCH_NAME --single-branch $REPOSITORY_URL
```

## Push master branch content to the new branch

In this example, you can populate the new branch with content from the master
branch using the command below.

> Note:


```sh
git push origin $NEW_BRANCH_NAME
```



## Delete the new branch on local file system

In this example, you can delete the branch using the command below.

> Note: This will not work if you are currently working on that branch. You
will need to checkout/switch to the master branch in order to delete that
branch from the local file system.

> Note: This branch deletion only occur on local file system and will only
affect the project/repository online source after the the current branch
structure has been pushed to the online source.

```sh
git branch -d $BRANCH_NAME
```

### EXAMPLE

```sh
git branch -d dev-T815873-mark-for-deletion
```

## Push the current branch structure back to GitLab

```sh
git push origin :dev-T815873
```