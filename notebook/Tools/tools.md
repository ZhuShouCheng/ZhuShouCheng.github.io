#### Windows快捷键

win->	左右分屏

alt space x	最大化窗口

alt space n	最小化窗口



### Shell基础

流	> 标准输出重定向，< 标准输入重定向，2> 标准错误流重定向。>/dev/null 表示将重定向内容丢弃。还可以使用 `>>` 来向一个文件追加内容。使用管道（ *pipes* ），我们能够更好的利用文件重定向。 `|` 操作符(pipe)允许我们将左边程序的输出和右边程序的输入连接起来	（有很多高级用法）

大小写	shell的参数大小写敏感

curl	client url	基础上是做url的访问，但可以通过各种参数实现其他功能

touch	用来修改文件时间戳，或者新建一个不存在的文件

tee	向文件和标准输出同时写入内容

cd -	- 指上一个使用的目录

mkdir my photo	会创建两个文件夹 my 和 photo	因此使用mkdir my\ photo or mkdir "my photo"

ctrl L	clear

ctrl C	可以用来pass掉写错的命令，进入下一行

ctrl ->	shell光标按单词左右移动

grep 命令用于查找文件里符合条件的字符串

cut	切割字符串	cut --delimiter=" " - f2

sudo	在使用|、>、< 时，需要操作符左右都有sudo	# su	$普通用户

xdg-open	用一个合适的应用打开file、url	Linux专用

rm -rf demo/    删除目录demo以及其下所有内容

shebang	#!即shebang，以指令`#!/bin/sh`开头的文件在执行时会实际调用*/bin/sh*程序。shell通过shebang了解使用什么方式解释代码。最好使用`#！/usr/bin/env python `这种形式来表明解释器

##### Shell编程
foo = bar	创建变量foo	注意空格

$foo	访问变量	可以在“$foo”中使用，但‘$foo’不行	

$1	shell函数的第一个参数	$0 shell程序的名称	$? 从上一条命令中获取错误代码	$_ 上一条命令的最后一个参数	$#当前函数的参数个数

source foo.sh   可以声明sh中的函数

!!	代指上一条命令

``` ||``` 	a || b 如果a执行失败，则执行b. a若成功，则b不执行	``` &&```正好相反

;	使用;可以在一行中执行多个语句

$(CMD)	命令替换	将命令变成变量，将CMD的输出结果替换掉$(CMD)	eg:(pwd)

<(CMD)	进程替换	执行CMD，并将结果输出到一个临时文件，并将<()替换成临时文件名

[[]]	使用双中括号将表达式括起来

-ne	not equal

通配符	*任意多个字符	？单个字符	{}当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令	eg:file{a,b,c}	== filea,fileb,filec

##### shell函数和脚本有如下一些不同点：

- 函数只能用与shell使用相同的语言，脚本可以使用任意语言。因此在脚本中包含 `shebang` 是很重要的。
- 函数会在当前的shell环境中执行，脚本会在单独的进程中执行。因此，函数可以对环境变量进行更改，比如改变当前工作目录，脚本则不行。脚本需要使用 [`export`](httsp://man7.org/linux/man-pages/man1/export.1p.html) 将环境变量导出，并将值传递给环境变量。
- 与其他程序语言一样，函数可以提高代码模块性、代码复用性并创建清晰性的结构。shell脚本中往往也会包含它们自己的函数定义。

#### Shell工具

命令帮助工具	tldr	命令示例	shellcheck	shell编程纠错工具

搜索文件工具	find	-name  -path  -type  -mtime	locate	类似everything

搜索字符工具	grep	ripgrep	fzf(模糊搜索)

搜索之前命令	history	ctrl R  倒序搜索含关键词的命令

目录显示	tree	broot	autojump



##### demo

find -name "*html" | xargs -d '\n'  tar -czvf html.zip	查找所有html文件，并压缩

find --help | grep -C 5 exec	查找find help文档中和 exec相关的5行

find . -type f -mtime -1 | xargs ls -lt	查找最近一天修改过的文件，并用按时间顺序打印

sleep 60 | wait && ls
#### 数据处理
使用正则表达式和流进行数据处理
##### 正则表达式
\w  正式字符，包括下划线
\s  空白
o{n}    多次匹配
.   占位符

##### 数据处理的常用命令
grep    查找行
sed -E 's/ //'  后面两个/中间写替换内容
sed -E 's/ (..) /\1/'   只保留第一个捕获组的内容，其他替换成空格
sort -nk1,1    排序
uniq -c 删除重复，并统计重复行
wc  统计数据

log show    mac打印日志


#### 命令行环境配置
##### 任务控制
<C-c>   SIGINT信号
<C-\>   SIGQUIT信号
kill -TERM <PID>  SIGNTERM  kill还可以通过参数的形式传输很多其他信号量
<C-z>   SIGSTP信号  可以使用fg(前台) bg(后台)来恢复进程
jobs 用于打印当前终端的未完成任务
&   后缀，用来让命令在后台执行


##### 终端多路复用
tmux

```
tmux
|--session 会话
|----window 窗口
|------Pane  面板
```

会话 - 每个会话都是一个独立的工作区，其中包含一个或多个窗口
tmux 开始一个新的会话
tmux new -s NAME 以指定名称开始一个新的会话
tmux ls 列出当前所有会话
在 tmux 中输入 <C-b> d ，将当前会话分离
tmux a 重新连接最后一个会话。您也可以通过 -t 来指定具体的会话
<C-b> : 进入命令格式
kill-session  kill-window  kill-pane

窗口 - 相当于编辑器或是浏览器中的标签页，从视觉上将一个会话分割为多个部分
<C-b> c 创建一个新的窗口，使用 <C-d>关闭
<C-b> N 跳转到第 N 个窗口，注意每个窗口都是有编号的
<C-b> p 切换到前一个窗口
<C-b> n 切换到下一个窗口
<C-b> , 重命名当前窗口
<C-b> w 列出当前所有窗口
面板 - 像 vim 中的分屏一样，面板使我们可以在一个屏幕里显示多个 shell
<C-b> " 水平分割
<C-b> % 垂直分割
<C-b> <方向> 切换到指定方向的面板，<方向> 指的是键盘上的方向键
<C-b> z 切换当前面板的缩放
<C-b> [ 开始往回卷动屏幕。您可以按下空格键来开始选择，回车键复制选中的部分
<C-b> <空格> 在不同的面板排布间切换

##### 别名
alias   别名，alias ll=”ls -al“
ln -n   建立符号链接

##### 远程连接
ssh foo@xxx.xxx.xxx.xxx
ssh 可以直接跟命令 让命令在远程设备上执行，可以配合grep等
ssh-copy-id 创建密钥连接SSH
~/.ssh/config   用这个文件来配置ssh的连接信息

sshfs   可以将远程服务器挂载到本地

### Git版本控制
#### Git数据模型
##### 模型以及实现
git将文件分成Blob（数据对象/文件）以及tree（目录）
```
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```
tree的结构是嵌套的，而最顶层的root即为整个项目

```
// 文件就是一组数据
type blob = array<byte>

// 一个包含文件和目录的目录
type tree = map<string, tree | blob>

// 每个提交都包含一个父辈，元数据和顶层树
type commit = struct {
    parent: array<commit>
    author: string
    message: string
    snapshot: tree
}
```
每一个元素都通过hash获得其ID，用来标志该对象
所有对象包含都通过哈希值引用实现
提交的哈希引用再通过map映射为字符串，便于阅读

Git仓库仅存储着 对象 和 引用

##### 逻辑结构
git的每一次提交都构成一个节点，每个节点都指向其父节点
git采用来有向无环图来表示逻辑结构
banch   分支
暂存区  需要提交的代码需要先放入暂存区中，再一并提交

#### Git命令行接口
##### 基础
git help <command>: 获取 git 命令的帮助信息
git init: 创建一个新的 git 仓库，其数据会存放在一个名为 .git 的目录下
git status: 显示当前的仓库状态
git add <filename>: 添加文件到暂存区
git commit: 创建一个新的提交
如何编写 良好的提交信息!
为何要 编写良好的提交信息
git log: 显示历史日志
git log --all --graph --decorate: 可视化历史记录（有向无环图）
git diff <filename>: 显示与暂存区文件的差异
git diff <revision> <filename>: 显示某个文件两个版本之间的差异
git checkout <revision>: 更新 HEAD 和目前的分支

##### 分支和合并
git branch: 显示分支
git branch <name>: 创建分支
git checkout -b <name>: 创建分支并切换到该分支
相当于 git branch <name>; git checkout <name>
git merge <revision>: 合并到当前分支
git mergetool: 使用工具来处理合并冲突
git rebase: 将一系列补丁变基（rebase）为新的基线

##### 远端操作
git remote: 列出远端
git remote add <name> <url>: 添加一个远端
git push <remote> <local branch>:<remote branch>: 将对象传送至远端并更新远端引用
git branch --set-upstream-to=<remote>/<remote branch>: 创建本地和远端分支的关联关系
git fetch: 从远端获取对象/索引
git pull: 相当于 git fetch; git merge
git clone: 从远端下载仓库

##### 撤销
git commit --amend: 编辑提交的内容或信息
git reset HEAD <file>: 恢复暂存的文件
git checkout -- <file>: 丢弃修改

##### Git 高级操作
git config: Git 是一个 高度可定制的 工具
git clone --depth=1: 浅克隆（shallow clone），不包括完整的版本历史信息
git add -p: 交互式暂存
git rebase -i: 交互式变基
git blame: 查看最后修改某行的人
git stash: 暂时移除工作目录下的修改内容
git bisect: 通过二分查找搜索历史记录
.gitignore: 指定 故意不追踪的文件
git config --global core.excludesfile ~/.gitignore_global

##### git教程
https://git-scm.com/book/zh/v2


###  调试及性能分析

#### 调试代码

##### 使用日志
- 您可以将日志写入文件、socket 或者甚至是发送到远端服务器而不仅仅是标准输出；
- 日志可以支持严重等级（例如 INFO, DEBUG, WARN, ERROR等)，这使您可以根据需要过滤日志；
- 对于新发现的问题，很可能您的日志中已经包含了可以帮助您定位问题的足够的信息。

##### 调试器
python pdb
c/c++ gdb pwndbg 
strace(Linux)\dtruss(macOS) 可以用来调试二进制程序
tcpdump\Wireshark   用来分析网络数据包

##### 静态分析工具
不需要执行代码即可找到问题  基于编码规则分析
pyflakes\mypy   python静态代码工具
shellcheck  shell静态分析工具
大多IDE都集成了这一功能，称为code linting

代码格式化工具  python-black Go-gofmt ...

#### 性能分析

##### 计时
通过在代码中添加计时函数实现时间统计

##### CPU性能分析工具

##### 内存性能分析工具

##### 资源监控
htop  通用监控


### 系统构建
#### make
```
<target> : <prerequisites> 
[tab]  <commands>
```
##### target
目标通常是文件名，指明Make命令要构建的对象
.PHONY  伪目标
    声明为伪目标后，make不会检查该目标是否存在，因此make clean会在任何情况下都执行

##### prerequisites 前置条件
前置条件通常是一组文件名，中间用空格分隔。它指明了目标是否重新构建都标准，即前置条件是否更新

##### commands 命令
命令用来表示如何构建目标文件，有shell语句组成。命令前必须有Tab键，每行命令在不同shell中执行，因此如果想要使用shell变量只能放在同一行，中间用逗号隔开，或者逗号后使用“\”转义，然后换行。

##### 语法

回声@   正常情况下，make会打印所有命令.命令前标注@，make将不会打印该命令

模式匹配%   %.o: %.c    对于目录中main.c
    即为  main.o: main.c

自动变量
    $@  指代当前目标
    $<  指代第一个前置条件
    $?  指代比目标更新对所有前置条件
    $^  指代所有前置条件，之间以空格分隔
    $*  指代匹配符%的内容
    $(@D/F) 指代当前目标的目录名和文件名


#### cmake

```
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
# 项目信息
project (Demo3)

# 定义变量
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

# 添加第三方依赖库的include、lib文件夹
include_directories(${CMAKE_SOURCE_DIR}/ext/jsoncpp/include)
link_directories(${CMAKE_SOURCE_DIR}/ext/jsoncpp/lib)

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
# 添加 math 子目录
add_subdirectory(math)
# 指定生成目标  使用所有源文件
add_executable(Demo $(DIR_SRCS))
# 添加链接库
target_link_libraries(Demo kmsjsoncpp)
```

1. add_library
该指令的主要作用就是将指定的源文件生成链接文件，然后添加到工程中去。该指令常用的语法如下：


add_library(<name> [STATIC | SHARED | MODULE]
            [EXCLUDE_FROM_ALL]
            [source1] [source2] [...])
其中<name>表示库文件的名字，该库文件会根据命令里列出的源文件来创建。而STATIC、SHARED和MODULE的作用是指定生成的库文件的类型。STATIC库是目标文件的归档文件，在链接其它目标的时候使用。SHARED库会被动态链接（动态链接库），在运行时会被加载。MODULE库是一种不会被链接到其它目标中的插件，但是可能会在运行时使用dlopen-系列的函数。默认状态下，库文件将会在于源文件目录树的构建目录树的位置被创建，该命令也会在这里被调用。

2. link_directories
该指令的作用主要是指定要链接的库文件的路径

3. target_link_libraries
该指令的作用为将目标文件与库文件进行链接。该指令的语法如下：


target_link_libraries(<target> [item1] [item2] [...]
                      [[debug|optimized|general] <item>] ...)
上述指令中的<target>是指通过add_executable()和add_library()指令生成已经创建的目标文件。而[item]表示库文件没有后缀的名字。默认情况下，库依赖项是传递的。当这个目标链接到另一个目标时，链接到这个目标的库也会出现在另一个目标的连接线上。这个传递的接口存储在interface_link_libraries的目标属性中，可以通过设置该属性直接重写传递接口。


#### 持续集成CI(Continuous integration)

https://www.ruanyifeng.com/blog/2015/09/continuous-integration.html

