# 学习 Shell

在上一章我们简单了解了 Shell，接下来让我们学习 Shell 的使用吧！

## 快捷键

Shell 也有快捷键？是的。大多数 Shell 都支持以下的快捷键：

- `C-C`：终止当前进程。
- `C-Z`：暂停当前进程。
- `C-D`：退出当前Shell（实际上是发送一个 `EOF` 文件结束字符）。
- `C-L`：清屏。

> 其中 `C-C` 和 `C-D` 是十分重要的两个快捷键，十分常用。

在 Bash 中，还有一些特殊的快捷键，它们继承自 GNU Emacs 编辑器，用于快速移动光标以及编辑文本：

- `C-A`：移动光标到行首。
- `C-E`：移动光标到行尾。
- `C-B`：向左移动光标。
- `C-F`：向右移动光标。
- `C-K`：删除从光标位置到行尾的文本。
- `C-U`：删除从光标位置到行首的文本。
- `C-W`：删除光标左侧的单词。
- `C-Y`：粘贴之前删除的文本。

> 这部分只需要了解即可

## 命令

命令分为内部命令和外部命令，内部命令是 Shell 自带的命令，外部命令是 Shell 外部的可执行程序。

一条 Shell 的指令的标准格式为：

```bash
command [options] [arguments]
```

其中，`command` 是命令的名称，`options` 是命令的选项，`arguments` 是命令的参数。

它的格式不一定必须遵守，但是通常遵守这个格式会带来更好的可读性（用什么工具采用什么方式对什么文件做什么）。

但是，有一个例外，就是指定输出，这时，我们推荐將指定输出的选项以及参数放在文件明后面。

例如，`ls` 命令用于列出当前目录下的文件和文件夹，它的选项可以是 `-l`、`-a` 等，参数可以是文件名或目录名。

## 变量

Shell 中的变量分为局部变量、系统变量和全局变量，系统环境变量是 Shell 启动时自动设置的变量，局部变量和全局变量是用户自定义的变量。

### 创建（定义）变量

我们可以用 `=` 创建一个局部变量，例如：

```bash
GREET="Hello, World!"
```

> 这里注意等号的两边都不要有空格

变量可以通过 `export` 命令设置为全局变量，例如：

```bash
export GREET="Hello, World!"
```

然后我们在这个 Shell 里启动一个新的 Shell，仍然可以在环境中找到这个全局变量。而一般变量就找不到了。

```bash
bash -c 'echo $GREET'
```

> `export` 声明的变量只在当前环境下有效，如果你开启了个新的终端或者重启，那么这个环境就无效了

变量也可以通过 `declare` 命令声明，例如：

```bash
declare USER_GREET="Hello, Shell"
```

> declare 默认声明的变量是局部的，和直接使用等号一样。使用 `-g` 可以设置为和 `export` 一样的全局变量。

我们可以使用 `declare -n` 可以显示定义的全部变量。

### 变量的作用范围（作用域）

我们执行以下命令：

```bash
# 最外层的 Bash1

bash # 启动 Bash2
a=1
export b=2
declare c=3
echo "Bash2 a:${a} b:${b} c:${c}" # 观察更改前 Bash2 中变量的值

bash # 启动 Bash3
echo "Bash3 a:${a} b:${b} c:${c}" # 观察更改前 Bash3 中变量的值
a=4 # 更改 Bash3 中的变量
export b=5
declare c=6
echo "Bash3 a:${a} b:${b} c:${c}" # 观察更改后 Bash3 中变量的值
exit # 退出 Bash3

echo "Bash2 a:${a} b:${b} c:${c}" # 观察更改后 Bash2 中变量的值
exit # 退出 Bash2

echo "Bash1 a:${a} b:${b} c:${c}" # 观察 Bash1 中变量的值
```

会输出：

```text
Bash2 a:1 b:2 c:3
Bash3 a: b:2 c:
Bash3 a:4 b:5 c:6
Bash2 a:1 b:2 c:3
Bash1 a: b: c:
```

我们会发现，使用等号定义的变量在里面的 Bash 获取不到，但是 export 获取的变量就能够访问。还有，在里面的 Bash 无论如何都影响不到外面的变量。

### 使用变量

变量可以通过 `${}` 或 `$` 符号来引用，例如：

```bash
echo ${GREET}
echo $USER_GREET
```

> 当变量的前后有别的内容紧挨着，那么只能使用 `${}`。

如果使用的变量不存在，那么程序不会报错，会默默的把这个变量 **替换为空值**，这点需要十分注意。

> 之前，有个软件的卸载用的 Shell Script 有一个 bug，会运行 `rm -rf $(INSTALL_PATH)/*`，但是这里的 `INSTALL_PATH` 变量可能会未定义，最终导致了清除了整个根目录。

### 系统变量

Shell 中有一些系统变量，它们在 Shell 启动时自动设置，例如：

- `$HOME`：当前用户的主目录。
- `$PATH`：可执行文件的搜索路径。
- `$SHELL`：当前Shell的名称。
- `$PWD`：当前工作目录。
- `$USER`：当前用户的用户名。

其中最重要也是最常用的就是 PATH，它决定了 Shell 在执行命令时搜索可执行文件的路径。当我们发现明明安装了某个软件但是找不到可执行文件的时候应该首先检查 PATH 的设置。如果你熟悉 Windows，你会发现 Windows 的 PATH 是一串由分号分隔的路径。而 Linux 下的路径使用的是 `:` 冒号分隔。

> 在 Windows 中的 PATH 中有一条路径是 `.` 就是当前目录，而 Linux 中默认是不会搜索当前目录的。如果你在当前目录有一个 `run.sh`，那么你必须要使用 `./run.sh` 才能执行。

另外还有以下几个特殊的系统变量：

- `$?`：上一个运行的程序的返回值（一个数字，通常 0 表示成功，其它数字表示错误，不同程序不同错误的返回值一般不同）
- `$0`：启动当前终端程序或者 ShellScript 的命令，如 `/bin/bash`
- `$#`：启动当前终端程序或者 ShellScript 的参数个数。

## 进程

在学习上一章节的时候，你是否会疑惑：为什么执行一个新的 Shell 就可以在一个不影响原本 Shell 的环境中工作呢？

实际上，我们在执行一个命令的时候，Shell 会为这个程序开启一个新进程。那什么叫做进程呢？你可以把它理解为你给计算机派发的一个任务。你只需要派发任务就好了，而计算机要考虑的事情就多了。（划掉）计算机负责执行这个进程，操作系统给它分配系统资源。

我们每个人都有自己的父母，进程也是一样。例如，进程 A 启动了进程 B，那么进程 A 就是进程 B 的父进程（也可以称之为母进程）。

一般情况下，进程的环境变量只能向下传递。也就是当父进程启动了一个子进程的时候，子进程会继承父进程的环境变量。但是这时集成的只是父进程在创建子进程时环境变量的一个快照，以后父进程中环境变量更新的时候不会影响子进程。

我们可以通过实例来了解一下这个问题。

```sh
$ export TEST=114514
$ echo $TEST
114514
$ bash # 启动一个新的 Shell 进程
$ echo $TEST
114514 # 从父进程那里继承了环境变量
$ export TEST=1919810 # 尝试修改变量
$ echo $TEST
1919810
$ exit # 回到父进程
$ echo $TEST
114514 # 父进程的环境变量没有被改变
```

## 管道

Shell 中的管道是链接两个命令的方式，管道遵守下面的格式。

```bash
command1 | command2
```

`command1` 的输出将会作为 `command2` 的输入提供，例如：

```bash
ls -a | grep "test"
```

这个命令将会列出当前目录下的所有文件，然后使用 `grep` 命令过滤出文件名包含"test"的文件。

## 输出重定向

Shell 中的重定向是改变命令的输出地点的方式，重定向遵守下面的格式。

```bash
command > file
```

`command` 的输出将会被重定向到 `file` 中，例如：

```bash
ls -a > files.txt
```

这个命令将会列出当前目录下的所有文件，然后使用 `>` 符号将输出重定向到 `files.txt` 中。

## 输入重定向

Shell 中的输入重定向是改变命令的输入地点的方式，输入重定向遵守下面的格式。

```bash
command < file
```

`command` 的输入将会被重定向到 `file` 中，例如：

```bash
cat < files.txt
```

这个命令将会读取 `files.txt` 中的内容，然后使用 `cat` 命令输出到标准输出中。

## 将命令的输出作为参数

让我们想像这个场景：有一个文件有一个文件名列表，我们想要获取这些文件的详细信息，这怎么实现呢？

运算符 `$()` 可以将命令的输出作为参数，它的标准格式如下：

```bash
command1 $(command2)
```

`command2` 的输出将会作为 `command1` 的参数。

实现前文的场景所需的命令如下：

```bash
ls -l $(cat files.txt)
```

这个命令将会读取 `files.txt` 中的文件名列表，然后使用 `ls -l` 命令获取这些文件的详细信息。

## 课后作业

1. 使用 Shell 的输出重定向功能，在主目录中创建一个保存了主目录中所有文件列表的文件，命名为 `files.txt`
2. 使用 `cat` 读取所有文件的内容。

---

> study-area-cn
