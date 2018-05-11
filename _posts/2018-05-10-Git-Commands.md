---
title: Git Commands
date: 2018-05-09 10:08:29
categories:
- Git
---

一次性上手Git。

# 初级：
进入工程文件夹后，右键->git bash here，在shell中
初始化本地仓库;
```
$git init
Initialized empty Git repository in D:/Unity_Project/SimplePatrol/.git/
```

查看本地仓库状态
```
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .vs/
        Assets/
        Library/
        ProjectSettings/
        SimplePatrol.csproj
        SimplePatrol.sln
        Temp/
        UnityPackageManager/

nothing added to commit but untracked files present (use "git add" to track)

```

将文件加入本地仓库（单个工程，不需要本地分支）
```
$ git add .
```

提交
```
$ git commit -m "framework"
```

创建并切换到本地分支
```
$ git checkout -b patrol
Switched to a new branch 'patrol'
```

**关联/移除关联远程分支** 可以通过仓库的url
```
$ git remote add origin <url/>
```
例如我添加的是"Unity-Game-Programming"
**获取远程分支**
```
$ git fetch
warning: no common commits
remote: Counting objects: 941, done.
remote: Compressing objects: 100% (463/463), done.
remote: Total 941 (delta 584), reused 810 (delta 460), pack-reused 0
Receiving objects: 100% (941/941), 5.92 MiB | 45.00 KiB/s, done.
Resolving deltas: 100% (584/584), done.
From https://github.com/WuYuQi0301/Unity-Game-Programming
 * [new branch]      Interaction-Hit-UFO -> origin/Interaction-Hit-UFO
 * [new branch]      Priests-and-Devils  -> origin/Priests-and-Devils
 * [new branch]      SimplePatrol        -> origin/SimplePatrol
 * [new branch]      Space-and-Motion    -> origin/Space-and-Motion
 * [new branch]      TicTacToe           -> origin/TicTacToe
 * [new branch]      hit-ufo             -> origin/hit-ufo
 * [new branch]      master              -> origin/master

```
# Problems
1. 向本地仓库添加文件时报错文件锁的存在无法添加
```
error: open(".vs/SimplePatrol/v15/Server/sqlite3/db.lock"): Permission denied
error: unable to index file .vs/SimplePatrol/v15/Server/sqlite3/db.lock
fatal: adding files failed
```
.lock文件被很多的操作系统和应用程序所使用来锁住某些资源，比如一个文件或者一个设备。典型的一般是没有包含任何数据的一个空的文件，但是可能也包含lock文件的属性和设置。Lock文件表明一个应用程序中某个资源在锁释放之前是不能被应用的。这对那些需要并发访问临界资源的应用程序是十分有用的。  
关闭vs和unity，删之。
2. push报错
```
$ git push origin SimplePatrol
error: src refspec SimplePatrol does not match any.
error: failed to push some refs to 'https://github.com/WuYuQi0301/Unity-Game-Programming'
```
原因：没有commit
