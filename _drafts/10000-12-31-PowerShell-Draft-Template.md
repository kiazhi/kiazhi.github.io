---
layout: single
title: "PowerShell Draft Template"
excerpt: "This is a PowerShell Draft Template"
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-07-22 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - PowerShell
  - "Ryen Tang"
---

Imagine Mickey Mouse created a branch and created a `CONTRIBUTING.md` markdown file with his name listed under the `CONTRIBUTORS` header.

```sh
git checkout -b Mickey_Mouse_Branch
echo "# CONTRIBUTORS" > CONTRIBUTING.md
echo "- Mickey Mouse" >> CONTRIBUTING.md
git add CONTRIBUTING.md
git commit -m "Added Mickey Mouse to CONTRIBUTORS list"
git push -u origin Mickey_Mouse_Branch
```

And Minnie Mouse came along, listing all the remote branches to check if any new branches were created and found `Mickey_Mouse_Branch` in the list. 

```sh
git branch -rv
```

**Output:**

```text
  origin/HEAD                -> origin/master
  origin/Mickey_Mouse_Branch 9d96c20 Added Mickey Mouse to CONTRIBUTORS list
  origin/master              f7ac036 changes made to master branch
```

Minnie Mouse decided to check out the `Mickey_Mouse_Branch` and found a new `CONTRIBUTING.md` markdown file with Mickey Mouse listed under the CONTRIBUTORS header.

```sh
git checkout -b Mickey_Mouse_Branch
cat CONTRIBUTING.md
```

Minnie Mouse then decided to add herself after Mickey Mouse name, staged the modified file and pushes the changes in the branch to the remote repository.

```sh
echo "- Minnie Mouse" >> CONTRIBUTING.md
git add CONTRIBUTING.md
git commit -m "Added myself to CONTRIBUTORS list"
git push -u origin Mickey_Mouse_Branch
```

Goofy found that `Mickey_Mouse_Branch` branch too and decided to play a prank on the same file in that branch.

```sh
git checkout -b Mickey_Mouse_Branch
rm CONTRIBUTING.md
echo "# CONTRIBUTORS" > CONTRIBUTING.md
echo "- Goofy (goofing around without Mickey Mouse & Minnie Mouse)" >> CONTRIBUTING.md
git add CONTRIBUTING.md
git commit -m "Updated the CONTRIBUTORS list"
git push -u origin Mickey_Mouse_Branch
```
git stash
git merge origin/Mickey_Mouse_Branch


