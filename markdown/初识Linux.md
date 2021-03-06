# 初识Linux
作者：忆红叶为妆
## Linux命令
**命令格式**


``` command[-options][parameter1]... ```
- command:命令名，相对功能的英文单词或者单词的缩写
- [-options] : 选项，可用来对命令进行控制，可以省略，[]代表可选
- parameter1... : 传给命令的参数，可以是零个、一个或者多个；

`ls` 命令用来显示目标列表，在Linux中式使用率较高的命令。

`ls-a` 显示所有文件（包含隐藏文件）

`ls-l` 以长格式显示目录下的内容列表。输出的信息从左到右一次包括文件名、文件类型、权限模式、硬连接数、所有者、组、文件大小（字节单位）和文件的最后修改时间等；

`ls-h` 人性化显示。文件大小显示为常见的大小单位 `KB` `MB` `GB` 等

多个参数可以放在一起写

```[test@ubntu~]$ ls -alh ```

在 `Ubuntu` 中，可以使用 `ls -al` 的别名 `ll` :

``` [test@ubuntu ~]$ ll ```

**查看帮助**


如果不清楚一个命令的作用和都有哪些可用的参数，可以使用命令帮助。一般都是 `命令 --help` ，比如：

```[test@ubuntu ~]$ ls --help ```

对于没有提供 `--help` 的命令，可以使用 `man` 帮助手册：

```[test@ubuntu ~]$ ls --help```

**几个常用个的命令**

- `clear` 清除当前屏幕终端上的任何信息，也可以使用 `ctrl + l`
- `history` 显示指定书目的历史命令，`history 10` 显示最近10条的历史命令
- `ctrl + c` 强制终止

**小技巧**

自动补全：

Linux系统使用 `tab` 键可以进行命令补全,自动填充后面的文件夹名称，自动填充后面的文件夹名称，如果有重复的名字，无法补缺，就按两次 `tab` 键，会告诉你到底需要补全哪一个，同理，命令也是会补全的

历史命令：

当系统执行过一些命令后，可以按 `↑↓` 键翻看以前的命令。

使用 `history` 可以查看历史命令，使用 `!` 加历史命令中的序号，可以快速执行命令：

```
[test@ubuntu ~]$ hisroty
[test@ubuntu ~]$ !1
``` 

## Ubnutu目录结构说明

**显示颜色说明**

白色：表示普通文件

绿色：表示可执行文件

红色：表示压缩文件

黄色：表示设备文件

蓝色：表示文件夹

浅蓝色：表示链接文件

**根目录结构说明**

Linux系统的目录结构遵循统一的标准，不同的系统会有一些细微的差别。

`/` 根目录

`/bin` 命令保存目录（普通用户就可以读取的命令）‘

`/boot` 存放Ubuntu内核和系统启动文件。系统启动时这些文件被先挂载。

`/cdrom` 光驱文件系统挂载目录

`/dev` 设备文件保存目录

`/etc` 配置文件保存目录

`/home` 普通用户的家目录

`/lib` 函数库的保存位置

`/lost+found` 包含了系统修复时的回复文件

`/media` 主要用于挂载多媒体设备

`/mnt` 此目录主要是作为挂载点使用

`/opt` 给第三方软件放置的目录

`/proc` 系统内存的映射

`/root` 超级用户的家目录

`/run` 系统在运行时所需文件，下次启动会重新生成，以前的位置是在 `/var/run` ,现在 `/var/run` 是指向 `/run` 的一个软连接

`/sbin` 命令保存目录（超级管理员才能使用的目录）

`/snap` ubuntu 全新的软件包管理方式

`/srv` 服务启动后要访问的数据目录

`/sys` 跟 `proc` 一样虚拟文件系统

`/tmp` 存放临时文件目录，可以随时读写

`usr` 应用程序方式目录

`/var` 存放系统执行过程经常改变的文件，包含日志文件、计划任务和邮件等内容

# 目录操作命令
## 目录

 目录是一组相关文件的集合
 
 一个目录下面出了可以存放文件之外还可以存放其他目录，即可包含子目录。
 
 在确定文件、目录位置时，DOS和Linux否采用 `路径名 + 文件名` 的方式。路径反应的是目录与目录之间的关系。
 
 Linux路径由到达定位文件的目录组成。在Linux系统中组成路径的目录分隔符为斜杠 `/`，而 DOS 则用反斜杠 `\` 来分割各个目录。
 
 ## 相对路径和绝对路径
 
 **绝对路径**
 
 绝对路径从目录树的树根：“/” 目录开始往下直至到达文件所经过的所有节点目录。
 
 下级目录接在上级目录后面用 `/` 隔开。
 
 绝对路径都是从 `/` 开始的，所以第一个字符一定是 `/` 
 
 **相对路径**
 
 相对路径是指定目标相对于当前目录的位置
 
 `.` 表示当前目录
 
 `..` 表示上一级目录
 
 ## 切换工作目录
 
 `pwd` 命令可以查看当前所在的目录：
 
 ```
 [test@ubuntu ~]$ pwd
 ```
 
 > `~` 代表当前用户的家目录

`cd` 命令可以进行目录切换

在使用命令行操作linux系统时，需要频繁更滑工作目录。 `cd` 命令可以帮助用户切换

> linux所有的目录和文件名大小写敏感，在切换目录的时候注意大小写

`cd` 后面可跟绝对路径，也可以跟相对路径。如果省略目录，则默认切换到当前用户的主目录

在进行目录切换的时候，如果要使用绝对路径，目录必须以 `/` 开始，否则默认使用先对路径

回到用户家目录:

```
[test@ubuntu ~]$ cd
```

`~` 表示家目录, 所以 `cd ~` 可以切换到用户家目录

`.` 表示当前目录, 所以 `cd .` 就是切换到当前目录

`..` 表示上一级目录, 所以 `cd ..` 就是切换到上一级目录

`cd -` 可以切换到上一次访问的目录

> 在切换目录的时候，要善于使用 `tab` 键，可以自动补全目录名

## 创建目录

`mkdir` 创建目录：

```
[test@ubuntu ~]$ mkdir test
```

在指定目录下创建新目录

```
[test@ubuntu ~]$ mkdir ./test/a
```

`mkdir -p` 递归创建目录：

```
[test@ubuntu ~]$ mkdir -p a/b/c
```

> 新建目录的名称不能与当前目录中已有目录、文件同名。切目录创建者必须对当前目录具有写权限

## 删除目录

`rmdir` 可以删除一个空目录，如果目录里面有文件或者其他目录，就不能删除了

`rm` 可以删除文件和目录，但是只能删除空目录

如果要删除一个非空的目录，可以使用 `rm -r` 递归删除该目录和该目录下的所有文件和目录：

```
[test@ubuntu ~]$ rm -r a
```

当删除一个不存在的目录是，会提示 `rm: cannot remove 'a': No such file or directory` ,使用 `rm -f` 可以强制删除，从而忽略提示：

```
[test@ubuntu ~]$ rm -rf a
```

`*` 表示所有的文件，如果想要删除当前目录下的所有文件，可以使用命令`rm -rf *` ，或者 `rm -rf ./*`

再删除目录的时候，切记确认好目录再删除，尤其是使用 `rm -rf ./*` 的时候，如果你少输入了`.` ,name就会从 `/` 根目录开始删，如果你恰巧使用的是管理员账户，那么整个系统都会被删除掉

尽量避免使用 `rm -rf ./*` ，可以使用 `rm -rf *` 来删除当前目录下的所有文件

更安全的是使用 `rm -i` 交互式删除，每删除一个文件都要确认一下，输入 `y` 回车确认删除，不需要删除的，输入 `n` 跳过。

## 复制目录和文件

`cp` 命令可以复制目录和文件，格式是：

```
[test@ubuntu ~]$ cp <源目录> <目标目录>
```

将源文件复制为目录文件，或者将源文件复制到目标目录。多个源文件使用空格分隔：

```
[test@ubuntu ~]$ cp b b.bak
```

如果赋值目录，要使用 `-r` 参数：

```
[test@ubuntu ~]$ cp -r a c
```

如果目标位置，已经存在同名文件或者目录，可以使用`-f` 参数，强制覆盖：

```
[test@ubnuntu ~]$ cp -rf a c 
```

`-v` 参数可以显示复制过程

常用参数

- `-a` 相当于 -dpr参数
- `-d` 保留链接
- `-f` 强制复制，覆盖目标文件
- `-i` 覆盖时询问用户
- `-p` 保留修改时间和访问权限
- `-r` 递归赋值（目录 => 目录）
- `-l` 创建链接
- `-v` 显示过程

建议在修改一些系统配置文件的时候，先把原来的系统配置文件赋值一份，再去修改原来的配置文件，防止改错了无法恢复：

```
[test@ubuntu ~]$ cp xx.conf xx.conf.bak
```

## 重命名/移动目录和文件

`mv` 命令可以移动目录，在移动的时候也可以进行重命名，格式是：

```
[test@ubuntu ~]$ mv <源目录> <目标目录>
```

移动文件：

```
[test@ubuntu ~]$ mv a /tmp/a
```

移动文件的同时重命名：

```
[test@ubuntu ~]$ mv a /tmp/a b
```

重命名的操作实际就是在当前目录移动目录

```
[test@ubuntu ~]$ mv b a
```


常用参数：

- `-f` 禁止交互式操作，如果有覆盖也不会给出提示
- `-i` 确认交互方式操作，如果mv操作将导师制对已经存在的目标问价的覆盖，系统会询问是否重写，要求用户回答以避免误覆盖文件
- `-v` 显示移动进度

# 文件操作命令

## 文件系统

在 Linux 系统中，一切都是文件。

Linux采用了树状结构的文件系统，他有目录和目录下的文件一起构成。

对文件的操作，无非就是 `增删改查` 

## 添加文件

**`touch` 创建文件**

`touch` 命令可以创建一个文件，如果这个问价已经存在，会修改文件的最后一次访问时间：

```
[test@ubuntu ~]$ touch a.txt
```

**编辑器创建文件**

使用编辑器编辑一个不存在的文件，保存之后会创建一个新的问价

## 删除文件

`rm` 命令

## 修改文件

**修改文件内容**

使用 `nano` 编辑器可以编辑文件：

```
nano a.txt
```

输入内容之后，`strl + o` 保存，回车确认，在案 `ctrl + x`退出编辑

**修改文件名**

`mv` 命令可以修改文件名，也可以移动文件的位置

## 查看文件

`cat` 命令可以查看文件内容，文件内容会输出到终端中，所以不适合查看大文件：


```
[test@ubuntu ~]$ cat a.txt
```

`cat -n` 查看内容的同时，显示行号

`more` 命令查看文件内容，当内容过多的时候，可以分屏显示，按空格进行翻页，把回车往下一行，按 `q` 退出显示：

```
[test@ubnuru ~]$ more ~/.bashrc
```

也可以使用 `less` 命令，与 `more` 的区别是，`less` 借助 `↑↓` 进行上一行和下一行

## 硬链接和软连接

`ln`命令可以建立连接文件（类似于windows中的快捷方式），连接文件分为软连接、硬链接两种

**硬链接**

创建硬链接：

ln <源文件> <链接文件>

```
[test@ubuntu ~]$ ln a.txt b.txt
```

硬链接文件和源文件之间具备同步功能

硬链接只能连接普通文件，不能连接目录

**软连接链接**

创建软连接：

ln -s <源文件> <链接文件>

```
[test@ubuntu ~]$ ln -s a b
```

源文件删除则软连接时效，源文件搬移亦可能造成链接失效；

链接文件的移动也可能导致软连接失败，所以推荐使用绝对路径创建软连接，这样一定链接文件不会导致链接文件失效；

```
[test@ubuntu ~]$ ln -s /home/test/a /home/test/b
```

## 其他相关命令

**`cp`命令，可以复制文件**

```
[test@ubuntu ~]$ cp a.txt b.txt
```

**`file`命令，可以查看文件详情**

```
[test@ubuntu ~]$ file a,txt
```

**`du`命令，计算文件或目录占用的空间**

```
[test@ubuntu ~]$ di a.txt
```

参数：

- `-h`参数人性化显示。自动以G、M、K为单位显示占用空间大小
- `-a`显示当前目录子目录中的文件
- `-c`显示文件数