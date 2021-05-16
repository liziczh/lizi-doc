---
title: Git | QuickStart
comments: true
date: 2018-04-30 23:56:55
id: git-quickstart
tags: 
- Git
categories: Git
toc: true
reward: true
copyright: true
---

<!--# Git QuickStart-->

Git 是目前世界上最流行的分布式版本控制系统。它速度快，简单，采用完全分布式，可高效管理代码，对非线性开发模式提供强力支持。

<!--more-->

Git 诞生于一个极富纷争的年代。起因是 Linus 创建了开源的 Linux，在 2002 年以前，Linux 的内核维护工作主要是由世界各地的志愿者将源码文件通过 diff 方式发给 Linus，然后由Linus本人手工合并代码。到了 2002 年，Linux 代码库之大已经很难通过手工方式管理，于是 Linus 选择了分布式版本控制系统 BitKeeper 管理和维护代码。2005 年，BitKeeper 与 Linux开源社区的合作关系结束，迫使 Linux 开源社区不得不开发一套属于自己的版本控制系统。于是，Linus 花了两周时间用 C 写了一个分布式版本控制系统Git，一个月之内 Linux 源码已经由 Git 进行管理了。

## 0. Git 基本操作

### 0.1 创建一个新的仓库 

```shell
$ git init
$ git add .
$ git commit -m "initial commit"
$ git remote add origin <url>
$ git push origin master
```

### 0.2 记录每次更新到远程仓库 

```shell
$ git add .
$ git commit -m "initial commit"
$ git push origin master
```

## 1. Git 配置

### 1.1 配置用户信息

```shell
# git配置用户名
$ git config --global user.name "username"
# git配置用户邮箱
$ git config --global user.email useremail@example.com
```

### 1.1 查看配置信息

```shell
$ git config --list
```

## 2. Git 本地仓库

### 2.1 创建本地仓库

```shell
# git初始化本地仓库
$ git init
```

将当前所在目录初始化为一个本地仓库。初始化完成后，在当前目录下会出现一个名为 `.git` 的目录，所有 Git 需要的数据和资源都存放在这个目录中，切记不要手动修改该目录下的任何文件。

### 2.2 克隆远程仓库到本地

```shell
# 克隆远程仓库的默认分支
$ git clone <url>
# 克隆远程仓库的指定分支
$ git clone -b <branch-name> <url>
```

> git支持多种数据传输协议：
>
> - ssh协议：`user@server:/path.git`
> - https协议： `http(s)://` 
> - git协议： `git://` 

## 3. Git 版本控制

**git 工作流程**：

![git版本控制](http://p6uturdzt.bkt.clouddn.com/git-filestream.png)

### 3.1 查看文件状态

```shell
# 查看仓库文件状态
$ git status
```

**git 文件状态变化**：

![git文件状态](http://p6uturdzt.bkt.clouddn.com/git-filestatus.png)

### 3.2 暂存修改

`git add` ：①跟踪未跟踪文件，②将已跟踪文件放入暂存区。

```shell
# 跟踪所有文件，暂存所有修改
$ git add .
# 跟踪某个文件，暂存某个文件修改
$ git add <file-name>
```

### 3.3 撤销文件修改

```shell
# 撤销文件修改
$ git checkout --<file>
```

### 3.4 提交更新

`git commit` ：将本次更新全部提交到本地仓库。

```shell
# 提交已暂存更新
$ git commit -m "提交说明"
# 暂存并提交更新
$ git commit -am "提交说明"
```


### 3.5 查看提交日志

```shell
# 查看提交日志
$ git log
```

### 3.6 版本回退

**1. 软回退**：只回退commit信息。

```shell
$ git reset --soft HEAD^
```

**2. 默认回退**：回退commit和index信息，只保留本地源码。

```shell
$ git reset --mixed HEAD^
```

**3. 硬回退**：commit、index信息、本地源码全部回退。

```shell
$ git reset --hard HEAD^
```

> 版本的表示方式：
> - `HEAD^` 上一版本，`HEAD^^` 上上个版本 ...... 
> - `HEAD~n`：上n个版本
> - `<commit_id>`：某个指定版本

## 4. Git 远程仓库

### 4.1 添加远程仓库

```shell
# 添加一个远程仓库
$ git remote add <remote-name> <url>
```

### 4.2 移除远程仓库

```shell
# 移除某个远程仓库
$ git remote rm <remote-name>
```

### 4.3 重命名远程仓库

```shell
# 重命名远程仓库
$ git remote rename <old-name> <new-name>
```

### 4.4 查看远程仓库

```shell
# 查看所有远程仓库
$ git remote -v
```

### 4.5 从远程仓库抓取数据

```shell
# 从远程仓库抓取数据
$ git fetch <remote-name>
```

### 4.6 推送数据到远程仓库

```shell
# 推送数据到远程仓库
$ git push <remote-name> <branch-name>
```

## 5. Git 分支

Git 分支原理：每一个分支存在一个分支指针，分支指针指向不同版本；
Git 分支切换：HEAD 指针指向某个分支指针；

### 5.1 创建分支

```shell
# 创建一个分支
$ git branch <branch-name>
```

### 5.2 切换分支

```shell
# 切换分支
$ git checked <branch-name>
#  创建并切换到该分支
$ git checked -b <branch-name>
```

### 5.3 合并分支

```shell
# 合并分支
$ git merge <other-branch>
```

### 5.4 删除分支

```shell
# 删除分支
$ git branch -d <branch-name>
```

### 5.5 查看分支

```shell
# 查看所有分支
$ git branch -v
```

## 6. GitHub Fork

Fork即派生项目。在GitHub社区中可以Fork任意开源仓库。Fork之后，GitHub 将在你的空间中创建一个项目副本，你对项目副本拥有读写权限。并且可以推送pull request给官方仓库贡献代码。

**Fork 流程**：

1. 从 master 分支中创建一个新分支
2. 提交一些修改到新分支来改进项目
3. 将这个分支推送到 GitHub 上
4. 创建一个合并请求(Pull Request)
5. 项目的拥有者合并或关闭你的合并请求

## 7. gitignore

.gitignore文件规范：

- `#`：注释
- `!`：取反
- `*`：任意长度字符
- `?`：匹配单个字符
- `[abc]`：匹配方括号中的任意单个字符
- `[0-9]`：匹配两个字符之间的任意字符
- `**`：匹配任意中间目录
- 以`/`开头防止递归
- 以`/`结尾指定目录



> 参考资料：[GitBook](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)；

