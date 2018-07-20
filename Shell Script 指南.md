# Shell Script 指南

[TOC]



## 什么是 Shell Script

Shell 不仅是一个功能强大的命令行接口，也是一个脚本语言解释器。

Shell Script 就像早期 DOS 系统的批处理文件（.bat），能够将许多 shell 指令汇整写在一起，搭配正则表达式、管道命令与数据流重定向等功能，帮助程序员简化日常的系统管理工作。

理论上说，所有解释型语言（Perl/Python/Php/Ruby/JavaScript...）都可以胜任脚本编程，编译型语言（C/Java/Ada/...）只要有解释器，也可以用作脚本编程。Shell Script 相比这些语言，速度较慢，且占用 CPU 资源较多，容易造成主机资源的分配不良，只适合小规模的处理工作。

## 如何编写 Shell Script

Shell Script 就是普通的文本文件，使用任何文本编辑器都能编写。

下面创建一个最简单的脚本 `~/bin/hello.sh`，在 Shell 中进行打印：

```shell
#!/bin/bash
# This is out first script.
echo 'Hello World!'
```

* 最后一行就是最简单的带有一个字符串参数的 `echo` 命令；

* 第二行是普通的注释，文本行中 `#` 符号之后的所有字符都会被忽略；

* `#!` 字符序列是一种特殊的结构叫做 shebang，用来告诉系统将执行此脚本所用的解释器的名字，每个 shell 脚本都应该把这一文本行作为第一行。

  > 注：如果在 shell 中执行脚本，默认用 shell 来解释，也就可以省略第一行。

根据脚本使用对象的不同，脚本的存放位置也不同：

* 如果脚本为个人所用，那最好把脚本存放在 `~/bin` 目录下；
* 如果脚本为系统中每个用户使用，那这个脚本的传统位置是 `/usr/local/bin`；
* 如果脚本只让系统管理员使用，应该存放在 `/usr/local/sbin` 目录下；
* 

## 如何执行 Shell Script

执行脚本有两种方式：

1. 作为解释器的参数执行：

   ```shell
   $ sh hello.sh
   $ bash hello.sh
   ```

   这种方式仅要求文件具备可读（r）权限。

2. 作为**可执行程序**直接执行：

   ```shell
   $ ./hello.sh		# relative path
   $ ~/bin/hello.sh	# absolute path
   $ hello.sh			# PATH
   ```

   > 注：一般情况下都要加上当前目录，如果不加目录，系统就会到 PATH 里去找。

   这种方式要求文件具备可读可执行（rx）权限：

   ```shell
   $ chmod +rx hello.sh
   ```

## 良好的编写习惯

为了避免遗忘，一定要养成良好的 Shell Script 编写习惯，在每个 `.sh` 文件头处记录好：

1. script 的功能；
2. script 的版本信息；
3. script 的作者与联络方式；
4. script 的版权宣告方式；
5. script 的历史记录；
6. script 内较特殊的指令，使用绝对路径的方式来下达；
7. script 运作时需要的环境变量预先宣告与设定。

除了记录这些信息之外，在较为特殊的程序代码部分，务必加上批注说明！

## 经典范例

### 对话式脚本

很多时候我们需要使用者输入一些内容，以让程序顺利运作。