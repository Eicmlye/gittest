# Git Usages

[toc]

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
2. `git` 命令只能在工作目录为 Git 版本库时才能使用, 否则报错

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
