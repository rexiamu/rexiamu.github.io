---
title: git的配置与使用
date: 2021-07-06 18:58:09
permalink: /pages/bec356/
categories:
  - 
tags:
  - 
---

[Git](https://git-scm.com/book/zh/v2) 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

## 下载安装

地址：[https://git-scm.com/downloads](https://git-scm.com/downloads)

安装好之后右击桌面或其他空白处，出现 `Git GUI Here` 和 `Git Bash Here` 这两个选项，选中 `Git Bash Here` 出现 git 命令窗口，后续的 git 命令在此执行。

执行 `git --version` 命令，出现版本号信息说明安装成功。

## 用户信息配置

设置用户名和邮件地址，这一点很重要，因为每一个 Git 提交都会使用这些信息。 `--global` 代表全局配置。

```bash
git config --global user.name "rexiamu"
git config --global user.email rexiamu@gmail.com
```

执行 `git config --list` 命令，出现用户名和邮件地址信息说明配置成功。

## 工作流程

![git工作流程](https://s2.loli.net/2022/05/31/QiFVCX8EDLH2fek.png)

区域说明：

- workspace：工作区。即我们所能看到的，编辑文件的地方。
- staging area：暂存区。每次的小修改我们可以先暂时放在这里，积累到一定程度时，再提交到本地仓库。
- local repository：本地仓库。git 分布式系统中我们在本地保存的仓库。
- remote repository：远程仓库。通常为放在代码管理平台上的仓库，许多人共同维护。

操作说明：

```bash
git add .                       # 提交修改到暂存区（. 表示当前目录下所有文件，提交指定文件时写成指定文件路径，如：./files/1.txt）
git commit -m 'update'          # 提交暂存区到本地仓库（'update':注释内容）
git push                        # 上传代码至远程仓库并合并
git pull                        # 拉取远程仓库代码并合并
git checket                     # 检出，创建分支、切换分支
git clone                       # 克隆远程仓库代码
```

## 常用操作
```bash
git init                        # 在当前目录创建仓库
git status                      # 查看文件状态
git diff                        # 查看未缓存的改变
git diff --cached               # 查看未提交的缓存
git commit -a -m 'update'       # 将修改提交到缓存并且提交到本地仓库（必须是已经跟踪过的文件）
git restore --staged .          # git add . 的逆向操作，将缓存内容清空
git log --oneline               # 查看 git commit 历史记录（--oneline：在一行显示）
```
