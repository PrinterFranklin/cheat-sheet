# Sublime Cheat Sheet

[TOC]

## 〇、前言

Sublime 是一个工具，其目的是提高程序员的生产效率，它的定位是作为一个轻量级的开发环境，胜任方便快捷的小规模开发。

我们之所以要对 Sublime 进行定制，是为了最大化发挥它的潜力以增进工作效率，而**盲目追求完善 Sublime 的功能是愚蠢的，因为付出的努力将远大于收获的回报**。

在适当的场合选择适当的工具，才是聪明人之所为。

## 一、打开方式

* 通过图标进行访问；
* 在 cmd 中使用 subl 进行访问（要先添加环境变量）。


## 二、安装 Package Control

安装 Package Control 有三种可行的方式：

* 通过 `Ctrl+~` 访问控制台，并输入以下代码加回车：

  ```python
  import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
  ```

  这段代码可以在 Package Control 官网找到：https://packagecontrol.io/installation。

* 在 Package Control 官网手动下载安装文件 Package Control.sublime-package，将其直接拷贝到 `Sublime Text3\Data\Installed Packages` 目录中。

  如果找不到这个目录，可以在 Sublime 界面中先 `Preference->Browse Packages`，再通过前一级目录 `Data` 进入 `Installed Packages`。

* 从 GitHub 上手动下载 Package Control 源码：https://github.com/wbond/sublime_package_control ，下载 `.zip` 文件后解压，将文件夹重命名为 Package Control，拷贝到 `Sublime Text3\Data\Packages` 目录中（就是 `Preference->Browse Packages` 的目录）。

无论采用上述哪种方式，都要重新启动 Sublime，`Ctrl+Shift+P` 打开命令面板看看能否正常使用 Install Package。

![](http://oxglnwe1z.bkt.clouddn.com/18-3-21/21008053.jpg)

## 三、通用插件&配置

Sublime 的配置文件和安装包存放路径位于 `Sublime Text3\Data` 下（就是 `Preference->Browse Packages` 的上一级目录）。

### 1. 必装插件

安装插件（Package）的方法是： `Ctrl+Shift+P` 进入控制台，输入 install Packages 回车，稍等一会后在输入框中搜索插件，选择安装即可。

1. ConvertToUTF8 插件

   Sublime 默认编码格式为 utf-8，若要支持显示 GBK, BIG5, EUC-KR, EUC-JP, Shift_JIS 等编码的内容，必须安装此插件。

2. Bracket

3. MarkdownPreview

### 2. 推荐设置



### 3. 使用 Git 备份插件与配置





## 四、特殊语境插件&配置

### 1. C/C++ 环境配置

####（1）下载安装 MinGW

Sublime 自带了 C/C++ 编译器，但自带的编译系统有以下不足：

- 程序的输出被捕获到 Sublime 窗口中，无法在运行时输入信息，执行 `scanf` 语句时会卡住。
- 默认情况下 C 和 C++ 没有区别，全部当作 C++ 格式处理。

如果要自定义 C/C++ 编译系统，需要首先安装 C/C++ 环境，其中最常用的是 MinGW。MinGW 是 Minimalist GNU on Windows 的首字母缩写，安装后就可以使用很多的GNU 工具。

GNU（GNU’s Not Unix）是linux中的一个著名的项目，包含了 gcc\g++\gdb 等工具。也就是说，安装 MinGW 后，我们就可以使用 `gcc` 和 `g++` 命令了。

安装方法就是**直接从 MinGW 的[官网](http://www.mingw.org/)下载安装**。

安装完成后需要**配置环境变量**（以装在 C 盘为例）：

1. 在 PATH 变量里加入 C:\MinGW\bin。
2. 新建 LIBRARY_PATH 变量，如果有的话，在值中加入C:\MinGW\lib，这是标准库的位置。
3. 新建 C_INCLUDEDE_PATH 变量，值设为 C:\MinGW\include。

安装后尝试使用 gcc 或 g++ 命令编译程序，如果成功，证明安装正确。

####（2）JSON 编译配置文件解释

Sublime 中自带的 C/C++ 编译器位于 `Sublime Text3\Packages` 目录下，将对应文件后缀名改为 `.zip` 后解压，可以找到名为 `C++ Single File.sublime-build` 的 JSON 配置文件：

```json
{
	"shell_cmd": "g++ \"${file}\" -o \"${file_path}/${file_base_name}\"",
	"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
	"working_dir": "${file_path}",
	"selector": "source.c, source.c++",

	"variants":
	[
		{
			"name": "Run",
			"shell_cmd": "g++ \"${file}\" -o \"${file_path}/${file_base_name}\" && \"${file_path}/${file_base_name}\""
		}
	]
}
```

这里面用到的名称（键）的含义如下：

| 名称        | 含义                                                         |
| ----------- | ------------------------------------------------------------ |
| working_dir | 运行cmd是会先切换到working_dir指定的工作目录。               |
| cmd         | 包括命令及其参数。如果不指定绝对路径，外部程序会在你系统的:const:PATH 环境变量中搜索。 |
| shell_cmd   | 相当于shell:true的cmd ，cmd可以通过shell运行。               |
| file_regex  | 该选项用Perl的正则表达式来捕获构建系统的错误输出到sublime的窗口。 |
| selector    | 在选定 Tools \| Build System \| Automatic 时根据这个自动选择编译系统。 |
| variants    | 用来替代主构建系统的备选。也就是一个配置文件可以对应多个执行命令。 |
| name        | 只在variants下面有，设置命令的名称，例如Run。                |

以上配置文件中还有一些类似 ${file} 这种符号，这是sublime提供的变量，常用的变量含义如下：

| 变量            | 含义                                               |
| --------------- | -------------------------------------------------- |
| $file_path      | 当前文件所在目录路径, e.g., *C:\Files*.            |
| $file           | 当前文件的详细路径, e.g., *C:\Files\Chapter1.txt*. |
| $file_name      | 文件全名（含扩展名）, e.g., *Chapter1.txt*.        |
| $file_extension | 当前文件扩展名, e.g., *txt*.                       |
| $file_base_name | 当前文件名（不包括扩展名）, e.g., *Document*.      |

这些变量可以直接使用，也可以使用花括号括起来，例如 `${project_name}`。

下面来详细阅读一下配置文件：

* ```json
  "working_dir": "${file_path}",
  ```

  这一行说明工作目录，即执行命令时所在的目录，被设置为文件所在的目录。

* ```json
  "shell_cmd": "g++ \"${file}\" -o \"${file_path}/${file_base_name}\"",
  ```

  这就是编译时执行的命令，执行时，`${file}` 被替换为编辑的文件名，`${file_path}/${file_base_name}` 被替换为不包含扩展名的完整路径名。其中，`\"` 是转义的双引号，双引号是为了支持带空格的文件名。

  如果文件路径是 `D:/project/hello.cpp`，那么 Windows 环境下实际执行的命令就成了：

  ```shell
  g++ "D:/project/hello.cpp" -o "D:/project/hello"
  ```

* 若有错误，错误信息会被 “file_regex” 中的正则表达式匹配并显示。

* ```json
  "variants":
  	[
  		{
  			"name": "Run",
  			"shell_cmd": "g++ \"${file}\" -o \"${file_path}/${file_base_name}\" && \"${file_path}/${file_base_name}\""
  		}
  	]
  ```

  variants 的值是一个数组，可以放很多对象，每个对象表示一个命令：`name` 表示这个命令的名称为 Run，即运行；`shell_cmd` 代表运行的具体命令，前半部分和之前一样，表示编译，`&&` 表示编译之后才执行的部分，即直接运行可执行文件。

####（3）自定义编译配置文件

选择 `Tool->Build System->New Build System`，为 C 和 C++ 分别配置编译文件：

* C ：

  ```json
  {
  	"encoding": "utf-8",
  	"working_dir": "$file_path",
  	"shell_cmd": "gcc -Wall \"$file_name\" -o \"$file_base_name\"",
  	"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
  	"selector": "source.c",
   
  	"variants": 
  	[
  		{	
  		"name": "Run",
          "shell_cmd": "gcc -Wall \"$file\" -o \"$file_base_name\" && start cmd /c \"\"${file_path}/${file_base_name}\" & pause\""
  		}
  	]
  }
  ```

  与默认配置相比，这里修改了 `selector` 部分为只选择 c 文件，并在 Run 中的 `shell_cmd` 后面部分加上了 `start cmd /c`，start 作用是新开一个 cmd 窗口，cmd 表示要执行一个命令行，/c 执行完后退出新开的窗口，后面的 & pause 保证运行结束后窗口不会立即退出。这样 Run 就会在新的 cmd 窗口中运行了。

  按 `Ctrl+s` 保存，会自动打开 `Sublime Text3\Data\Packages\User` 目录，修改文件名为 c.sublime-build，保存在此目录。

  此时，在 `Tools->Build System` 中就能看到新建的 c 了。

  > 这么做貌似还有个奇怪的 bug：直接 `Ctrl+B` 也会打开 cmd，如果把 variant 这一段注释掉就没事，为了保留运行功能，建议把 Run 换个名字，另设一个快捷键（我换成了 RunYourAss，设置 ctrl+alt+b）。

* C++：

  ```json
  {
  	"encoding": "utf-8",
  	"working_dir": "$file_path",
  	"shell_cmd": "g++ -Wall -std=c++11 \"$file_name\" -o \"$file_base_name\"",
  	"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
  	"selector": "source.c++",
   
  	"variants": 
  	[
  		{	
  		"name": "Run",
          "shell_cmd": "g++ -Wall -std=c++11 \"$file\" -o \"$file_base_name\" && start cmd /c \"\"${file_path}/${file_base_name}\" & pause\""
  		}
  	]
  }
  ```

  把 gcc 改成 g++，source.c 改成 source.c++，增加一条 -std=c++11，保存文件名改成 c++.sublime-build。

####（4）GDB 调试

事实上，安装完的 MinGW 里面就包含了 gdb 调试程序，只要在命令行访问即可：

```shell
$ gcc -g hello.c -o hello
$ gdb hello
GNU gdb (GDB) 7.6.1
Copyright (C) 2013 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "mingw32".
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>...
Reading symbols from D:\Project\2018\C\BubbleSort.exe...done.
(gdb)
```

命令行 gdb 调试中的一部分常用命令，仅供参考，可跳过不看：

| 命令              | 作用                                 |
| ----------------- | ------------------------------------ |
| list/l + (行号)   | 从某行开始显示源代码，一次列十行     |
| start             | 开始运行，停在变量声明后的第一条语句 |
| next/n            | 执行下一步，不进入函数（step over）  |
| step/s            | 进入函数内部（step into）            |
| backtrace/bt      | 查看函数调用栈帧                     |
| info/i + (locals) | 查看变量值，加 locals 参数表示当前栈 |
| print/p + 变量名  | 查看指定变量值                       |
| finish            | 跳出函数                             |
| frame/f + 帧编号  | 选择栈帧                             |

在命令行中调试相对麻烦，如果要**在 Sublime 中访问 gdb**，首先需要用 Package Control 安装 SublimeGDB 插件，然后修改目录 `Preference->Package Setting->SublimeGDB->Settings-User` 下的配置文件，输入以下代码：

```json
{
    "workingdir":"${folder:${file}}",
    "commandline":"g++ -g -std=c++11 ${file} -o ${file_base_name} && gdb --interpreter=mi --args ./${file_base_name}",
}
```

保存后重启。默认的快捷键（可以通过修改 Default.sublime-keymap 修改快捷键）如下：

| 按键      | 功能                            |
| --------- | ------------------------------- |
| F5        | 开始调试                        |
| Ctrl+F5   | 停止调试                        |
| F9        | 设置断点                        |
| F10       | Step over，执行一步，不进入函数 |
| F11       | Step into，进入函数             |
| Shift+F11 | Step out，跳出函数              |

在 GDB Callstack 点击可以跳转到对应函数处。在底部可以手动输入 gdb 命令进行调试。

使用感受是在 Sublime 里调试很不靠谱，特别卡，也无法进行输入。所以小文件中的错误还是手动找或者用 cmd 调试，大文件用 Visual Studio 即可。

#### （5）MakeFile 多文件编译 

Windows 环境下没这个必要！带来的麻烦远远胜过好处。

## 五、常用快捷键

### 0. 通用类

- `Ctrl+Z`：撤销
- `Ctrl+Y`：恢复撤销
- `Ctrl+A`：全选
- `Ctrl+C`：复制
- `Ctrl+X`：剪切
- `Ctrl+V`：粘贴
- `Ctrl+Shift+V`：自适应缩进的粘贴
- `Ctrl+N`：新建
- `Ctrl+Shift+N`：从新窗口新建
- `Ctrl+O`：打开文件
- `Ctrl+S`：保存
- `Ctrl+Shift+S`：另存为
- `Ctrl+W`：关闭标签页
- `Ctrl+Shift+W`：关闭窗口

### 1. 搜索类

* **`Ctrl+P`：任意跳转**

  * 键入文件名（的一部分）来打开文件；

  * **使用 `@` 搜索函数名，使用 `#` 搜索文件内任意内容，使用 `:` 跳到某一行。**

    也可以分别用 `Ctrl+R`、`Ctrl+:`、`Ctrl+G` 打开特定功能搜索框。

  以上指令还可以进行自由组合，例如，`tp@rf` 可以找到文件 textparser.py 的 read_file 函数，`tp:100`  可以跳到该文件的第 100 行。

* `Ctrl+Shift+P`：打开命令框，调用 Sublime 自带功能或插件功能（很适合选择文件格式）。

* `Ctrl+~`：打开控制台

* `Ctrl+F`：关键字搜索，支持正则表达式，但不能修改！

* `F3/Shift+F3`：搜索结果上下切换

* `Ctrl+H`：关键字替换

* `Ctrl+Shift+F`：在文件夹中查找（尚未研究）

* `Esc`：退出光标多行选择、搜索框、命令框等。

### 2. 选择类

* 多重选择
  * `Ctrl+D`：选中光标所在单词，继续操作会选中下一个相同单词
  * `Ctrl+K+D`：跳过一个词，选中下一个词
  * `Alt+F3`：选中某文本时，一次性选中所有相同文本同时编辑
  * `Ctrl+L`：选中整行，继续操作会选择下一行，效果与 Shift 一样
  * `Shift+方向键`：选中文本
  * `Ctrl+Shift+左右键`：以词为单位选中文本
  * `Ctrl+Shift+M`：选择括号内全部文本
* 光标
  * `Ctrl+左右键`：以词为单位移动光标
  * `CtrL+Alt+Up/Down`：添加多行光标（可配合进行矩形选择）
  * `Ctrl+Shift+L`：在每行行尾插入光标
  * `Ctrl+M`：将光标移至括号结束或开始的位置
* 书签
  * `Ctrl+F2`：在该行设置书签
  * `F2`：切换到有书签的行

### 3. 编辑类

* `Ctrl+/`：C++ 风格注释
* `Ctrl+Shift+/`：C 风格注释
* `Ctrl+Shitf+K`：删除整行
* `Ctrl+Shift+Up/Down`：移动整行或选中区域
* `Ctrl+Enter`：向下插入新行
* `Ctrl+Shift+Enter`：向上插入新行
* `Ctrl+Shift+[/]`：折叠/展开当前代码块
* `Ctrl+K+0`：展开所有折叠代码
* `Ctrl+J`：合并选中的代码为一行
* `Ctrl+Shift+J`：拆分
* `Tab`：缩进，可对选中集体操作
* `Shift+Tab`：回退
* `Ctrl+Shift+U`：转大写
* `Ctrl+Shift+L`：转小写

### 4. 显示类

* 分屏

  * `Alt+Shift+1`：默认 1 屏
  * `Alt+Shift+2`：左右分屏 - 2 列
  * `Alt+Shift+3`：左右分屏 - 3 列
  * `Alt+Shift+4`：左右分屏 - 4 列
  * `Alt+Shift+5`：等分 4 屏
  * `Alt+Shift+8`：垂直分屏 - 2 屏
  * `Alt+Shift+9`：垂直分屏 - 3 屏

* 切页面

  * `Ctrl+n`：组间切换（切分屏）
  * `Ctrl+Tab`：组内切换（按浏览顺序切同一分屏内的标签页）
  * `Ctrl+PageDown`：组内向右切换
  * `Ctrl+PageUp`：组内向左切换

* 其他

  * `f11`：全屏模式
  * `Shift+f11`：免打扰模式
  * `Ctrl+K+B`：开启/关闭侧边栏（注意先按 K）

  ​



