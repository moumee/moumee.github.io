---
title: "Azure Devops File Size Limit Fix"
date: 2025-07-29 00:00:00 +0900
categories: [Devlog, Git, Azure Devops]
tags: [git, azure devops]

---

I was working on a 1 vs 1 third person game using unreal engine 5 and had no problems with using Azure Devops since it provides large git lfs storage for free. But when I imported a lot of assets and tried to push, it put out an error saying **error: failed to push some refs to 'https://dev.azure.com/Moumee/ShatterDuel/_git/ShatterDuel'**.

And luckily I found the fix for this issue through a channel in visual studio developer community webpage. [https://developercommunity.visualstudio.com/t/cant-push-large-git-lfs-files-to-repository-from-p/862165](https://developercommunity.visualstudio.com/t/cant-push-large-git-lfs-files-to-repository-from-p/862165) 

It seems that git uses HTTP/2 protocol which limits the max file size of lfs objects to 128MB. With the following command below, you can set git to use the HTTP/1.1 which resolves the problem.

```Bash
git config http.version HTTP/1.1
```
