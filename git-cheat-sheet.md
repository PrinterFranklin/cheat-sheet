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

1. 在命令行中进入你要创建版本库的目录。
2. 在命令行中键入 `git init`，仓库就建好了，目录下会多一个 `.git` 隐藏目录。


## 三、添加文件

添加文件（必须是仓库目录/子目录下的文件）到仓库需要三步：

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

3. 时尚健康的零食

