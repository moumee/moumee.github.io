---
title: "Unreal Engine 5 Repository Contribution"
date: 2025-06-23 00:00:00 +0900
categories: [Devlog, Unreal Engine 5]
tags: [ue5, github]

---

Recently, my pull request for Unreal Engine 5 repository got accepted. I was studying UE5's Gameplay Ability System (GAS) and saw that the comments in AttributeSet.h had some minor typos. So I made a pull request of the UE5 repository fixing the typos in the header file.

![UE5 Pull Request](/assets/img/posts/2025-06-23/ue5_pull_request.png){: w="850" h="566" }
_UE5 repository pull request_

![UE5 Commit History](/assets/img/posts/2025-06-23/ue5_commit_history.png){: w="850 h="185 }
_UE5 repository commit history_

The pull request I made was actually kind of wrong. When I forked the UE5 repository, the default branch was set to 'release' so I accidentally created a branch from the 'release' not the 'ue5-main'. But thankfully, Epic games developers made a new commit in the ue5-main branch referencing my pull request.

Although my pull request may be a very trivial fix, I am very happy to contribute to one of the most used and famous game engines.
