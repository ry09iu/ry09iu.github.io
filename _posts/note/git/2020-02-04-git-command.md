---
layout: article
title: '[GIT 常用操作] 通过 git reflog 找回已删除的提交'
date: 2020-02-04 22:15
key: git_20200204_1
category: 
- note 
- git
tags:
- 学习笔记
- Git
---

### 前言
通过 GIT 管理的项目，如果想知道提交记录，可以通过`git log`命令来查看，但是，`git log`只能看到提交的记录，并不能看到完整的操作记录。那么，到底有没有方法可以达到这个目的？答案是当然有，那就是`git reflog`。

<!-- more -->

### 正文

通过`git log`我们可以看到之前提交的版本信息

```
commit 62fd1f6ed94d0110de51ca333fb6e314c6fed95f (HEAD -> new)
Author: Ry09iu <sxpatch@sina.cn>
Date:   Tue Feb 4 22:08:59 2020 +0800

    更新

commit b3310ad77544a2aec134033d642cc59cb157298d (master)
Author: Ry09iu <sxpatch@sina.cn>
Date:   Tue Feb 4 21:36:48 2020 +0800

    third

commit 8b83b9c437d2abac299ae891e3a2673fae003d6f
Author: Ry09iu <sxpatch@sina.cn>
Date:   Tue Feb 4 21:36:16 2020 +0800

    second

commit 938d668365ca7b7b10762a09e1efb9d6d1bfc9af
Author: Ry09iu <sxpatch@sina.cn>
Date:   Tue Feb 4 21:35:15 2020 +0800

    init
```

当然，你也可以加上`--pretty=oneline`查看简洁的信息

```
ry09iudeMacBook-Pro:demo ry09iu$ git log --pretty=oneline
62fd1f6ed94d0110de51ca333fb6e314c6fed95f (HEAD -> new) 更新
b3310ad77544a2aec134033d642cc59cb157298d (master) third
8b83b9c437d2abac299ae891e3a2673fae003d6f second
938d668365ca7b7b10762a09e1efb9d6d1bfc9af init
```

当使用`git rebase`命令删除掉某条版本提交后，`git log`命令将看不到这条提交的任何信息，但是`git reflog`可以，甚至包括中间的所有操作，这简直就是一台时光机啊！

```
f5c0498 (HEAD -> new) HEAD@{0}: rebase -i (finish): returning to refs/heads/new
f5c0498 (HEAD -> new) HEAD@{1}: rebase -i (pick): 更新
8b83b9c HEAD@{2}: rebase -i (start): checkout 938d668365ca7b7b10762a09e1efb9d6d1bfc9af
62fd1f6 HEAD@{3}: rebase -i (abort): updating HEAD
62fd1f6 HEAD@{4}: rebase -i (abort): updating HEAD
938d668 HEAD@{5}: rebase -i (start): checkout 938d668365ca7b7b10762a09e1efb9d6d1bfc9af
62fd1f6 HEAD@{6}: rebase -i (finish): returning to refs/heads/new
62fd1f6 HEAD@{7}: rebase -i (start): checkout 8b83b9c437d2abac299ae891e3a2673fae003d6f
62fd1f6 HEAD@{8}: commit: 更新
b3310ad (master) HEAD@{9}: checkout: moving from master to new
b3310ad (master) HEAD@{10}: commit: third
8b83b9c HEAD@{11}: commit: second
938d668 HEAD@{12}: commit (initial): init
```

比如，刚刚我删除了`third`这条提交，如果想找回来，只需要使用`git cherry-pick b3310ad`就可以恢复，绝对的后悔药一枚。