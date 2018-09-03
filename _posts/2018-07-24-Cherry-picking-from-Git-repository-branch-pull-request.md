---
layout: single
title: "Cherry picking from Git repository branch pull request"
excerpt: "What happen when there are too many cherries blooming on a Git
repository branch? Demonstrating how to pick those riped cherries from a
Pull Request."
author: Ryen Tang
header:
  image: ./assets/images/banners/Git-1280x200.png
date: 2018-07-24 00:00:00 +1200
toc: true
categories: 
  - Blog
  - Git
tags:
  - Blog
  - Git
  - "Ryen Tang"
---

Ever worked on a Git repository involving multiple people with an individual
that committed a crime to the repository where you need to scavenge the
working commit and discard the non-working ones?

I certainly have done something like this before and let me share this with you
on how you can cherry-pick or scavenge the working bits. In order to elaborate
this process better, I have provided a simple scenario that you can try and fix
it yourself.

## Scenario

Imagine Mickey Mouse created a branch and created a `CONTRIBUTING.md` markdown
file with his name listed under the `CONTRIBUTORS` header using PowerShell.

```sh
# Create a new branch to work on
git checkout -b Mickey_Mouse_Branch
```

```powershell
# Create content for a new markdown file
#  using PowerShell
"# CONTRIBUTORS" | `
  Out-File `
    -PSPath CONTRIBUTING.md `
    -Encoding utf8 ;

"- Mickey Mouse" | `
  Out-File `
    -PSPath CONTRIBUTING.md `
    -Encoding utf8 `
    -Append ;
```

```sh
# Add and stage the new file
git add CONTRIBUTING.md

# Commit the changes with a message
git commit -m "Added Mickey Mouse to CONTRIBUTORS list"

# Push the changes to remote branch
git push -u origin Mickey_Mouse_Branch
```

And Minnie Mouse came along, listing all remote branches to check if Mickey
Mouse's new branch has been created and found `Mickey_Mouse_Branch` in the
list.

```sh
# List the remote branches
git branch -rv
```

**Output:**

```text
  origin/HEAD                -> origin/master
  origin/Mickey_Mouse_Branch 9d96c20 Added Mickey Mouse to CONTRIBUTORS list
  origin/master              f7ac036 changes made to master branch
```

She checked out `Mickey_Mouse_Branch` and found the new `CONTRIBUTING.md`
markdown file with Mickey Mouse listed under the CONTRIBUTORS header.

```sh
# Check out the specific remote branch
git checkout -b Mickey_Mouse_Branch
```

```powershell
# Display the content of the file
#  using PowerShell
Get-Content `
  -Path CONTRIBUTING.md
```

Since she was told to add herself after Mickey Mouse name, she launches
PowerShell and modified the file using PowerShell.

```powershell
# Append the file with additional content
#  using PowerShell
"- Minnie Mouse" | `
  Out-File `
    -PSPath CONTRIBUTING.md `
    -Encoding utf8 `
    -Append ;
```

After her modification, she staged the modified file and pushes the changes in
the branch to the remote repository.

```sh
# Add and stage the appended file
git add CONTRIBUTING.md

# Commit the changes with a message
git commit -m "Added myself to CONTRIBUTORS list"

# Push the changes to remote branch
git push -u origin Mickey_Mouse_Branch
```

Next, Minnie Mouse rang Goofy and instructed him to do the same too. Goofy
checked out `Mickey_Mouse_Branch` branch, added his name to the list using Bash
and created a Pull Request.

```sh
# Check out the specific remote branch
git checkout -b Mickey_Mouse_Branch

# Display the content of the file
#  using Bash
cat CONTRIBUTING.md

# Append the file with additional content
#  using Bash
echo "- Goofy" >> CONTRIBUTING.md

# Add and stage the appended file
git add CONTRIBUTING.md

# Commit the changes with a message
git commit -m "Bashed Goofy to CONTRIBUTORS list"

# Push the changes to remote branch
git push -u origin Mickey_Mouse_Branch
```

> Note: In order to submit a Pull Request using command line, you will need to
install the [Hub](https://github.com/github/hub/releases) command line tool
from GitHub.

```sh
# Submit a Pull Request with a message
#  to GitHub
hub pull-request -m 'Updated the list and submitting PR'
```

Mickey Mouse got notified by an email, starts reviewing the Pull Request
submitted by Goofy and found that the last commit made by Goofy is not encoded
correctly in the Pull Request.

Instead of rejecting the Pull Request and inform Minnie and Goofy to do it
again, Mickey has two choices to resolve the issue:

1. Fetch the Pull Request to another branch and remove Goofy's recent commit
before closing the erroneous Goofy's Pull Request
2. Perform `git cherry-pick` on his commit and Minnie's commit before closing
the erroneous Goofy's Pull Request

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

## Problem solving walk-through

In this section based on the scenario above, I will elaborate on the two
options available for solving the problem.

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

### Removing recent commit in the Pull Request

If the erroneous commit is the most recent commit in the Pull Request, you can
do the following by fetching the Pull Request commits to a new branch and
remove the recent commit in that new branch. Once you are satisfied with it,
you make a new Pull Request from that new branch that only contains the commits
you wanted.

```sh
# Fetch the pull request to a new branch for fixing
# Syntax:
# git fetch origin pull/[Pull Request Number]/head:[New Branch Name] 
git fetch origin pull/1/head:PR1

# Checkout to the branch
git checkout PR1

# Display the commits history
git log

# Reset last recent commit in the history
git reset --hard HEAD~1

# Push the fixed branch to remote
git push -u origin PR1
```

Submit a new Pull Request from the `PR1` branch either using the site or
[Hub](https://github.com/github/hub/releases) command line developed for
GitHub.

```sh
# Submit a Pull Request with a message
#  to GitHub
hub pull-request -m 'Removed last commit in PR1 and resubmitting PR'
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

### Cherry picking commits from the Pull Request

If the erroneous commit is in between multiple other working commits, you can
create a new branch and `cherry-pick` those commits based on their SHA1 hash.
Once you are done with the cherry picking, you can create a new Pull Request
from the new branch and merge the Pull Request with the `master` branch.

```sh
# Create a new branch for cherry picking
git checkout -b Cherries_Bucket
```

```sh
# List commit ids that are ahead of the branch
git log HEAD...FETCH_HEAD
```

> Note: Commit ID is a long SHA1 hash and may be different from the example
below. To `cherry-pick`, you can either input the entire SHA1 hash or the
first 8 characters of the SHA1 hash.

```sh
# Pick those cherries based on the commit id
# Syntax:
# git cherry-pick [Commit Id]
git cherry-pick a481cc3c
```

Once you are done with picking those commits (cherries) from the Pull Request,
you can commit all those commits from this `Cherries_Bucket` branch and push
them to the remote repository.

```sh
# Add or stage all modified files and commit the changes with a message
git commit -a -m "Cherry picked the required commits"

# Push the changes to remote branch
git push -u origin Cherries_Bucket
```

Submit a new Pull Request from the `Cherries_Bucket` branch either using the
site or [Hub](https://github.com/github/hub/releases) command line developed
for GitHub.

```sh
# Submit a Pull Request with a message
#  to GitHub
hub pull-request -m 'PR containing cherry picked commits'
```

With your Pull Request submitted, you can begin to merge from the Pull Request

```sh
# Change to local master branch
git checkout master

# Merge fast-forward changes from local source branch
git merge --no-ff Cherries_Bucket

# Push the changes to the remote master branch
git push origin master
```

After you have merged the Pull Request to the `master` branch, you can delete
the remote and local `Cherries_Bucket` branch now.

```sh
# Delete the remote branch
git push origin :Cherries_Bucket

# Delete the local branch
git branch -d Cherries_Bucket
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

### Closing outstanding Pull Request containing the erroneous commit

In order for Mickey Mouse to close the Pull Request containing the erroneous
commit that Goofy had submitted, he will need delete the remote
`Mickey_Mouse_Branch` branch and the Pull Request will be marked as close.

```sh
# Delete the remote branch to close the Pull Request
git push origin :Mickey_Mouse_Branch
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

## Conclusions

In this blog post, you have learnt on how to use `git fetch`, `git reset` and
`git cherry-pick` skill to save those valid commits in a Pull Request without
merging those erroneous commit to the `master` branch.

All thanks to hot dog, hot dog, hot diggity dog mistakes.

If you happen to like what I am sharing, share this with others and keep them
informed.

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

- [Git-SCM Docs: Git fetch](https://git-scm.com/docs/git-fetch)
- [Git-SCM Docs: Git reset](https://git-scm.com/docs/git-reset)
- [Git-SCM Docs: Git cherry-pick](https://git-scm.com/docs/git-cherry-pick)
- [Github Hub Manual: GitHub Hub pull-request](https://hub.github.com/hub-pull-request.1.html)

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

<div id="amzn-assoc-ad-a6d31d8f-31aa-4399-9c4d-8b3c037007ab"></div><script async src="//z-na.amazon-adsystem.com/widgets/onejs?MarketPlace=US&adInstanceId=a6d31d8f-31aa-4399-9c4d-8b3c037007ab"></script>

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