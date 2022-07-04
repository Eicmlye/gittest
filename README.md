# Git 使用指南

1. [安装初始化](#1-安装初始化)
    - [1.1. 设置用户名和邮箱](#11-设置用户名和邮箱)
    - [1.2. 设置远程仓库 SSH Key](#12-设置远程仓库-ssh-key)
2. [版本库初始化](#2-版本库初始化)
3. [修改版本库](#3-修改版本库)
    - [3.1. 添加、修改文件](#31-添加、修改文件)
    - [3.2. 删除文件](#32-删除文件)
4. [发布修改](#4-发布修改)
5. [版本库状态](#5-版本库状态)
    - [5.1. 查询当前修改状态](#51-查询当前修改状态)
    - [5.2. 查询修改内容](#52-查询修改内容)
    - [5.3. 操作回退](#53-操作回退)
    - [5.4. 版本修改历史](#54-版本修改历史)
    - [5.5. Git操作历史记录](#55-Git操作历史记录)
    - [5.6. 版本回退](#56-版本回退)
6. [关联 GitHub 远程库](#6-关联-github-远程库)
    - [6.1. 本地库没有对应的远程库时](#61-本地库没有对应的远程库时)
    - [6.2. 新建项目创建版本库时](#62-新建项目创建版本库时)

---

## 1. 安装初始化

### 1.1. 设置用户名和邮箱

    $ git config --global user.name "USER_NAME"
    $ git config --global user.email "USER_EMAIL"

### 1.2. 设置远程仓库 SSH Key

在 `C:/User/USER_NAME` 目录中查看是否有 `.ssh` 目录. 若无, 执行

    $ ssh-keygen -t rsa -C "USER_EMAIL"

以创建 SSH Key.

**注意:** 切勿泄露私钥 `id_rsa`.

## 2. 版本库初始化

    $ cd REPO_PATH
    $ mkdir REPO_NAME
    $ cd REPO_NAME
    $ git init
    Initialized empty Git repository in /REPO_PATH/REPO_NAME/.git/

## 3. 修改版本库

**注意:**
1. 避免使用 Windows 自带的记事本软件编辑文本文件, 建议使用 VS Code 或 Atom.
2. `git` 命令 (`git init` 和 `git clone` 除外) 只能在工作目录为 Git 版本库时才能使用, 否则报错

        fatal: not a git repository (or any of the parent directories)

### 3.1. 添加、修改文件

将要添加到库中的文件移动到库目录内, 或将库中要修改的文件修改后,

    $ git add FILE_NAME

更新后的文件 `FILE_NAME` 即进入暂存区.

**注意:**
必须将 `FILE_NAME` 先移入 `REPO_NAME` 或其子目录, 再使用 `git add`, 否则报错

    fatal: pathspec 'FILE_NAME' did not match any files

### 3.2. 删除文件

将要删除的文件先移出库或直接删除, 然后执行

    $ git rm FILE_NAME

修改即进入暂存区.

## 4. 发布修改

执行

    $ git commit -m "MESSAGE"

暂存区的修改即被发布为最新版本库.

## 5. 版本库状态

### 5.1. 查询当前修改状态

    $ git status

1. 若显示

        On branch main
        Nothing to commit, working tree clean

    表明当前版本库未经修改, 与最近一次发布时状态相同.
2. 否则, 应显示

        On branch main
        Changes not staged for commit:
            ...
        Changes to be committed:
            ...
        Untracked files:
            ...

    以下修改不会被 `git commit` 发布:
    - 第一处省略号表示仍在工作区、但库中存在同名文件的修改;
    - 最后一处省略号表示仍在工作区、且库中不存在同名文件的修改;

    以下修改在 `git commit` 执行后会发布至最新版本库中:
    - 第二处省略号表示已提交至暂存区的修改.

### 5.2. 查询修改内容

    $ git diff FILE_NAME

### 5.3. 操作回退

撤销工作区中文件 `FILE_NAME` 的所有修改, 将其状态返回至最新版本库中的状态, 执行

    $ git checkout -- FILE_NAME

将暂存区中文件 `FILE_NAME` 的所有修改退回到工作区, 执行

    $ git reset HEAD FILE_NAME

### 5.4. 版本修改历史

    $ git log

由近到远显示各次修改的信息

    commit MD5_ID (HEAD->main)
    Author: USER_NAME <USER_EMAIL>
    Date:   DATE TIME TIMEZONE

        MESSAGE

### 5.5. Git操作历史记录

    $ git reflog

显示若干行

    MD5_ID (HEAD->main) HEAD@{NUM}: COMMAND: MESSAGE

### 5.6. 版本回退

Git 中, `HEAD` 为当前版本指针, 向前1个版本为 `HEAD^`, 向前 n 个版本为 `HEAD~n`.

1. 要回退到前一版本, 执行

        $ git reset --hard HEAD^

2. 要回退或在回退后要重新恢复到指定版本, 执行

        $ git reset --hard MD5_ID

    其中 `MD5_ID` 可在 `git reflog` 的执行结果中查找.

## 6. 关联 GitHub 远程库

将 [1.2.](#12-设置远程仓库-ssh-key) 中的公钥 `id_rsa.pub` 内容复制到 GitHub 账号 SSH Key, 并将 `USER_EMAIL` 设置为 GitHub 账号的**可见**邮箱. 然后执行相应操作.

### 6.1. 本地库没有对应的远程库时

在 GitHub 账号中创建同名版本库 `REPO_NAME`, 不要加入任何默认文件（包括开源协议、`.gitignore` 文件和 `README` 文件等）. 执行

    $ git remote add origin https://github.com/GITHUB_USER_NAME/REPO_NAME.git

将本地库与远程库关联. 执行

    $ git push -u origin main

将本地分支 `main` 同步到远程库. 参数 `-u` 将本地分支 `main` 与远程库分支 `main` 关联, 此后再次提交该分支无需此参数.

### 6.2. 新建项目创建版本库时

在 GitHub 账号中创建版本库 `REPO_NAME`, 加入 `README` 文件和开源协议. 将工作目录移至存放版本库的目录后, 执行

    $ git clone https://github.com/GITHUB_USER_NAME/REPO_NAME.git

本地将克隆远程库 `REPO_NAME` 的所有内容, 并将其初始化为 Git 版本库. 
