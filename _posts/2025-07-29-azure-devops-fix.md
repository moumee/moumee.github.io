---
title: "Azure Devops File Size Limit Fix"
date: 2025-07-29 00:00:00 +0900
categories: [Devlog, Git, Azure Devops]
tags: [git, azure devops]

---

While working on a project using Azure Devops and Git LFS, I tried to push a large amount of asset files but Git put out an error saying **error: failed to push some refs to 'https://dev.azure.com/Moumee/ShatterDuel/_git/ShatterDuel'**.

Luckily I found the fix for this issue through a channel in visual studio developer community webpage. [https://developercommunity.visualstudio.com/t/cant-push-large-git-lfs-files-to-repository-from-p/862165](https://developercommunity.visualstudio.com/t/cant-push-large-git-lfs-files-to-repository-from-p/862165)

It seems that git uses HTTP/2 protocol which limits the max file size of lfs objects to 128MB. With the following command below, you can set git to use the HTTP/1.1 which resolves the problem.

```Bash
git config http.version HTTP/1.1
```
