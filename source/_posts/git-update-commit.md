---
title: git修改已提交的commit注释
date: 2020-02-10
tags: ['git']
---

修改注释分为两种情况：
1. 代码未push到远程仓库， 还在本地仓库中。
2. 已经将代码push到远程仓库了。

第一种情况比第二种情况要多操作一步。

# 代码未push到远程仓库

## 修改最后一次注释

在项目路径下打开终端，输入：
`git commit --amend`

回车，出现界面如下，第一行是你的注释，输入`i`，进入编辑模式，将注释改好后按`Esc`退出编辑模式，输入`:wq`保存并退出，这时候你的注释就已经改好了。

![blockchain](/images/git-update-commit/01.png "修改最后一次注释")

## 修改之前的注释

1. 输入：
   `git rebase -i HEAD~2`

   最后的数字2指的是显示到倒数第几次， 比如2就会代表显示倒数的两次注释。
   如图：

![blockchain](/images/git-update-commit/02.png "修改之前的注释")

2. 你想修改哪条注释，就把哪条注释前面的`pick`换成`edit`，注意不要动注释内容，只要改前面的东西就好了。
   `i`进入编辑模式，把`pick`换成`edit`后，`Esc`退出编辑模式，`:wq`保存并退出。

3. 接下来输入：
   `git commit --amend`

   修改好注释内容后，输入：
   `git rebase --continue`

4. 修改结束。

## 修改之前某几次注释

第一步操作同上，第二步，你可以将多个想修改的commit注释前面的`pick`都换成`edit`。

接下来依次修改你的注释（顺序是从旧到新），Terminal基本都会提示你接下来的操作，每修改一个注释都要重复上面的3和4步，直到修改完你所选择的所有注释。

# 代码已经push到远程仓库

首先，`git pull`保证本地代码最新，修改的方法都如上，最后修改完成后，强制push到远程仓库：
`git push --force origin master`

**注：要保证在你强制push之前没有人提交代码，如果在你push之前有人提交了新的代码到远程仓库，然后你又强制push，那么会被你的强制更新覆盖！**

最后记得检查一下远程提交记录~

gitmoji：https://gitmoji.carloscuesta.me/

— END —
