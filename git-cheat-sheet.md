# Git Cheat Sheet

[TOC]

## 一、安装 Git

安装 Git 只需要三步：

1. 从 Git 官网或者国内镜像下载安装程序并安装。

2. 在命令行中输入 `git` 或 `git --version` 检查是否已经正确安装，

3. 设置本机 Git 用户名和邮箱，在命令行中键入：

   ```shell
   $ git config --global user.name "nbztx"
   $ git config --global user.email "nbztx@126.com"
   ```

## 二、创建仓库

创建一个版本库（repository）非常简单：

1. 从命令行进入你要创建版本库的目录。

2. 在命令行中键入 `git init`，仓库就建好了，目录下会多一个 `.git` 隐藏目录。

   ```shell
   $ git init
   ```


## 三、本地操作

### 1. 提交文件

提交文件（必须是仓库目录/子目录下的文件）到仓库需要两步：

1. 使用 `git add` 命令添加文件到仓库：

   ```shell
   $ git add readme.txt
   ```

   成功执行上述命令后没有提示。

2. 使用 `git commit` 把文件提交到仓库：

   ```shell
   $ git commit -m "wrote a readme file"
   ```

   其中，`-m` 和它之后输入的文本是 `git commit` 命令的参数，作为本次提交的说明。写提交说明是个好习惯，这样有助于从历史记录中方便地找到改动记录。

   成功执行上述命令后会提示 n 个文件被改动（可以多次 `add` 不同文件，用 `commit` 一次性提交）。

### 2. 查看仓库状态

* 如果仓库中的文件被修改了，或者有文件未被提交到仓库，使用 `git status` 命令可以让我们随时掌握仓库当前的状态：

  ```shell
  $ git status
  ```

* 如果要看看具体修改了什么内容，可以使用 `git diff` 命令：

  ```shell
  $ git diff
  ```

### 3. 版本控制

* 使用 `git log` 命令查看所有 `commit` 历史：

  ```shell
  $ git log
  commit 2bf4a614c916f7ab1e7df2c71a6b87d213f50616 (HEAD -> master)
  Author: nbztx <nbztx@126.com>
  Date:   Tue Mar 20 20:38:24 2018 +0800

      modifiy

  commit 3563f44fc4218417af3385f2cefb657f0e714079
  Author: nbztx <nbztx@126.com>
  Date:   Tue Mar 20 20:18:17 2018 +0800

      wrote git-cheat-sheet
  ```

  如果加上 `--pretty-oneline` 参数，可以看到版本号，这是一个用 SHA1 计算出来的非常大的数字，用十六进制表示：

  ```shell
  $ git log --pretty=oneline
  2bf4a614c916f7ab1e7df2c71a6b87d213f50616 (HEAD -> master) modifiy
  3563f44fc4218417af3385f2cefb657f0e714079 wrote git-cheat-sheet
  ```

* 使用 `git reset` 命令进行版本回退。

  在 Git 中 `HEAD` 表示当前版本，`HEAD^` 表示上一个版本，`HEAD^^` 表示上上个版本，`HEAD~50` 表示往上 50 个版本：

  ``` shell
  $ git reset --hard HEAD^
  ```

* 使用 `git reset` 命令重返未来！

  只需在命令之后加上版本号就能跳转到任意版本。

  ```shell
  $ git reset --hard 2bf4a6
  ```

  如果不记得版本号了，可以使用 `git reflog` 命令查看之前每一次命令的版本号。


### 4. 暂存区

一个 Git 目录可以分为两大部分：

* 工作区：除了 `.git` 文件夹外的整个文件夹。
* 版本库：隐藏目录 `.git` 中就是 Git 的版本库，里面存了很多东西，其中最重要的有：
  * 暂存区（stage）
  * master 分支
  * 指向 master 的一个指针 HEAD

执行 `git add` 命令就是**将工作区的文件修改添加到暂存区**，而 `git commit` 命令则是**将暂存区的所有内容提交到当前分支**。

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

Git 所跟踪并管理的是修改，而不是文件。如果不把文件 add 到暂存区中，文件的修改就不会提交到主分支。

### 5. 撤销修改

####（1）撤销工作区修改

可以使用 `git checkout -- file` 撤销工作区的修改：

* 如果文件修改后还没有 `add` 到暂存区（暂存区里东西），那么撤销修改就回到和版本库一模一样的状态。
* 如果文件之前有修改已经 `add` 到暂存区（尚未提交），又发生了修改，那就回到暂存区的状态。

> 注意：`git checkout` 后面的文件名参数很重要，不然命令将是另一种含义。

####（2）撤销暂存区修改

可以使用 `git reset HEAD file` 撤销暂存区修改， 将修改只留在工作区。

如果要彻底撤销该修改，再打一句 `git checkout -- file` 即可。

#### （3）撤销版本库修改

只需要回退版本，这在“版本控制”章节已经讲了，不再赘述。

### 6. 删除文件

如果要彻底删除一个文件，只从文件夹用 `rm` 删除是不够的，还要删除版本库里的对应文件：

```shell
$ git rm file
```

相反，如果文件被误删，版本库里还保留着最新版本，只需 `git checkout -- file` 便可复原（当然，最新的尚未被提交的修改还是丢失了）。

## 四、远程仓库

Git 是一个分布式版本控制系统，其精髓在于“**分布式**”三个字，即版本库是可以分布在不同机器上的，如果要协同工作，只要从其他机器上克隆版本库即可。

实际情况往往是这样的：有一台中心服务器，每天 24 小时开机，其他每个人都从服务器仓库上克隆一份代码到自己电脑上，各自把自己的修改提交给服务器仓库，也从服务器仓库拉取别人的提交。

这里以 GitHub 为例，介绍远程仓库的使用。

### 1. 创建并添加远程库

1. 创建 SSH Key（本地 Git 仓库和 GitHub 远程仓库之间的传输用 SSH 加密）

   ```shell
   $ ssh-keygen -t rsa -C "nbztx@126.com"
   ```

   这个 Key 的用处是帮助 GitHub 识别提交是否来自于你，当然，如果仓库有多个提交者，也可以添加多个  Key。一路回车，使用默认值即可，如果没有特殊用途，没有必要设置密码。

   如果一切顺利的话，可以在用户主目录下找到 `.ssh` 目录，里面有 id_rsa 和 id_rsa.pub 两个文件，这两个就是 SSH Key 的私钥和公钥。

2. 登陆 GitHub，在 `Settings -> SSH and GPG Keys` 页面中点击 `New SSH Key`，取个任意名字，粘贴上 id_rsa.pub 中的内容，确认即可。

3. 添加远程库。在本地仓库目录输入以下命令：

   ```shell 
   $ git remote add origin git@github.com:PrinterFranklin/cheat-sheet.git
   ```

   添加完成后，远程库的名字就是 origin，当然这个名字可以自己定，但 origin 更加形象、规范。

### 2. 向远程库推送

只需一步，就可以把本地内容全部推送到远程库上：

```shell
$ git push -u origin master
```

由于远程库是空的，第一次推送分支时要加上 `-u` 参数，这样不仅会把本地 master 分支推送到远程新的 master 分支，还会把本地 master 分支和远程 master 分支关联起来，使以后推送简化命令。

第一次推送之后，只要本地做了提交，就可以通过以下命令完成推送：

```shell
$ git push origin master
```

### 3. 从远程库克隆



