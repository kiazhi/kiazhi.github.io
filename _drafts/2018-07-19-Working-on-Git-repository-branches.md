### Cloning a branch from a project or repository to local file system

In this example, you can clone an existing project or repository from the
online source to your local file system using the following command
below.

```sh
git clone -b $BRANCH_NAME --single-branch $REPOSITORY_URL
```

### Creating a new branch on local file system

In this example, you can create a new branch for the project/repository in your
local file system using the following command below.

> Note: This example will only create an empty branch on local file system.

> Note: The new branch will not exist in the project/repository online
source until the branch is pushed to the online source.

```shell
git checkout -b $NEW_BRANCH_NAME
```


## Validate your current working branch locally

In this example, you can check the branch that you are currently working on
using the following command below.

### SYNTAX

```shell
git branch
```

## Push master branch content to the new branch

In this example, you can populate the new branch with content from the master
branch using the command below.

> Note:

### SYNTAX

```shell
git push origin $NEW_BRANCH_NAME
```

## Switch between branch

In this example, you can switch from the current working branch to the master
or other branches using the command below.

### SYNTAX

```shell
git checkout {branch name}
```

### EXAMPLE

```shell
git checkout master
```

## Delete the new branch on local file system

In this example, you can delete the branch using the command below.

> Note: This will not work if you are currently working on that branch. You
will need to checkout/switch to the master branch in order to delete that
branch from the local file system.

> Note: This branch deletion only occur on local file system and will only
affect the project/repository online source after the the current branch
structure has been pushed to the online source.

### SYNTAX

```shell
git branch -d {branch name}
```

### EXAMPLE

```shell
git branch -d dev-T815873-mark-for-deletion
```

## Push the current branch structure back to GitLab

```shell
git push origin :dev-T815873
```