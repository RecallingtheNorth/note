## Vim 介绍



Vim 是全屏幕纯文本编辑器，是vi编辑器的升级版。



vim 不仅仅兼容vi所有的命令，而且高亮显示，vi只能运行在 UNIX 和 Linux 中，而 vim 可以跨平台运行在Windows，MAC OS 中。



vim 号称 “编辑器之神”，具有无可匹敌的可扩展性。



大部分 Linux 系统中，默认都会安装 vim，在 Linux 中使用 vim 比较简单，直接使用 `vim` ，命令即可：



```
[user@ubnutu ~]$ vim a.txt
```



在命令模式下，使用 `:wq` 可以保存退出



## 三种模式

 

vim 共分为三种模式，分别是命令模式 (Command mode)，输入模式 (Insert mode)  和底线命令模式 (Last line mode) :

![](http://cdn.wkbook.cn/img/20200916151659.png)



**三种模式的切换**



一开始进入vim的时候，就是命令模式，在命令模式下，使用一些插入按键，就会进入输入模式，比如 `i` 键。



在输入模式下，按 `ESC` 键退出输入模式



在命令模式下，输入`:` 进入底线命令模式



命令模式下，输入 `:wq`保存并退出编辑，也可以输入 `ZZ` (shift + zz) 保存退出



**命令模式**



一开始进入 vim 的时候，就是命令模式，在命令模式下，敲击键盘动作会被识别为命令，而非输入字符。



命令模式下常用的一些命令：



|      命令      |                             说明                             |
| :------------: | :----------------------------------------------------------: |
|       :        |                       进入底线命令模式                       |
|       i        |              前插入模式，从选择字的前面开始插入              |
|       I        |                      在本行行首进行插入                      |
|       a        |              后插入模式，从选择字的后面开始插入              |
|       A        |                      在本行行尾进行插入                      |
|       o        |                 下插入模式，从下一行开始插入                 |
|       O        |    上插入模式，在此行的上面一行重新另外起一行新的空白插入    |
|       r        |                    替换当前光标所在的字符                    |
|       R        |        从光标所在字符字符开始替换, 直到按 `ESC` 退出         |
| h或左箭头键(←) |                     光标向左移动一个字符                     |
| j或下箭头键(↓) |                     光标向下移动一个字符                     |
| k或上箭头键(↑) |                     光标向上移动一个字符                     |
| l或右箭头键(→) |                     光标向右移动一个字符                     |
|       G        |                        移动到最后一行                        |
|       nG       | 移动的到指定的行, n为行数, 可以配合 `:set nu` 使用, 和 `:n` 作用一样 |
|       gg       |                         移动到第一行                         |
|      /str      | 向光标之下搜索 `str`, 此模式下, 按 `n` 继续向下搜索, 按 `N` 向上搜索 |
|      ?str      | 向光标之上搜索 `str`, 此模式下, 按 `n` 继续向下搜索, 按 `N` 向上搜索 |
|       x        |             删除光标所在字符, 即向后删除一个字符             |
|       X        |                       向前删除一个字符                       |
|       nx       |   n为数字, 向后删除指定数量的字符, 如 `5n` 向后删除5个字符   |
|       dd       |                   删除当前光标所在的这一行                   |
|      ndd       | 从光标所在行开始数, 向下删除 n 行, 如 `10dd`, 向下删除10行(包含光标所在行) |
|       yy       |                     复制光标所在的那一行                     |
|      nyy       | 从光标所在行开始数, 向下复制 n 行, 如 `10yy`, 向下复制10行(包含光标所在行) |
|       p        |             将已复制的内容粘贴到光标所在的下一行             |
|       P        |             将已复制的内容粘贴到光标所在的上一行             |
|       u        |                             撤销                             |
|    Ctrl + r    |                            反撤销                            |
|       .        |                        重复前一个动作                        |
|       ZZ       |                    保存退出, 相当于 `:wq`                    |
|       ZQ       |                 不保存强制退出, 相当于 `:q!`                 |



**输入模式**



在命令模式下，只要按下i，o，a等字符就可以了进入输入模式了，终端左下角显示为 `–INSERT-`



再输入模式中，任意按键都被当做字符串进行输入



按 `ESC` 键可以退出输入模式，回到命令模式



**底部命令模式**



在命令模式下，按冒号键 `:` （英文冒号） 就进入了底线命令模式。



底线命令模式可以输入一个或者多个字符的命令



`ESC` 可以退出底线命令模式，回到命令模式



底线命令模式常用命令：



|                      命令                      |                             说明                             |
| :--------------------------------------------: | :----------------------------------------------------------: |
|             :n1,n2s/word1/word2/g              |  `n1` `n2`是行号, 把 `n1`行到 `n2` 行之间的word1替换为word2  |
|  `:1,$s/word1/word2/g` 或 `:%s/word1/word2/g`  |            从第一行到最后一行, 将word1替换为word2            |
| `:1,$s/word1/word2/gc` 或 `:%s/word1/word2/gc` | 从第一行到最后一行, 将word1替换为word2, 每次替换都会进行确认, `y` 确认替换, `n` 跳过替换 |
|                       :n                       |                       光标移动到第几行                       |
|                    :set nu                     |                           显示行号                           |
|                   :set nonu                    |                          不显示行号                          |
|                       :w                       |                             保存                             |
|                       :q                       |                           退出vim                            |
|                      :wq                       |                          保存并退出                          |
|                      :q!                       |                  放弃未保存的编辑, 强制退出                  |
|                   :! command                   |                暂时离开vim 查看命令的执行结果                |





##  关机和重启



关机命令：



```
[user@ubuntu ~]$ shutdown -h now
```



重启命令：



```
[user@ubuntu ~]$ sudo shutdown -r now

[user@ubuntu ~]$ sudo reboot
```



> 该命令需要管理员权限，所以加上 `sudo`



## 时间和日期



显示当前时间：



```
[user@ubuntu ~]$ date
```



格式化当前时间：



```
[user@ubuntu ~]$ date "+%Y-%m-%d %H:%M:%S"
```



- `%Y` 年份(以四位数来表示)
- `%m` 月份(以01-12来表示)
- `%d` 日期(以01-31来表示)
- `%H` 小时(以00-23来表示
- `%M` 分钟(以00-59来表示)
- `%S` 秒(以本地的惯用法来表示)



设置当前日期和时间



```
[user@ubuntu ~]$sudo -s "20201112 13:40"
```



注意：只有管理员才能修改时间



当修改完时间，再次使用 `date` 查看时间，可以看到时间并没有改变，这是因为 `Ubuntu 20.04` 开启了 `NTP` 自动更新：



``` 
[user@ubuntu ~]$ timedatectl show
```



![](https://s3.ax1x.com/2020/11/12/Bx6QZ4.png)



关闭 `NTP` 自动更新：



```
[user@ubuntu ~]$ sudo timedatectl set-ntp no
```



关闭 `NTP` 自动更新后，再次设置时间就会生效



> 除非你有特殊的需要，否则不建议关闭 `NTP` 自动更新



开启 `NTP` 自动更新:



```
[user@ubuntu ~]$ sudo timedatectl set-ntp yes
```



Linux 系统模式使用的格林威治时间,也就是中央时区,北京位于东八区,也就是北京的地方时比中央时区的地方时早8小, 所以要设置一下时区, 以符合我们的使用



设置东八区时区:



```
[user@ubuntu ~]$ sudo timedatectl set-timezone Asia/Shanghai
```



> `timedatectl list-timezones`  命令可以显示所有的时区列表, 空格往下翻页, `q` 退出 .
>
> ![](https://s3.ax1x.com/2020/11/12/BxgIx0.png)



## 输出重定向



一些命令的输出，可以通过 `>` 或者 `>>` 重定向到一个文件中。



> 重定向，就是原本命令执行的结果是输出到终端的，通过 `>` 或者 `>>` 输出到某个文件里面了



格式是：



```
[user@ubuntu ~]$ command >> file.log
```



如果没有这个文件，会自动创建一个文件



`>` 会覆盖文件中的所有内容(相当于重新生成了一个文件)



`>>` 不会覆盖，会在文件的最后一行新增一行，同时统计的修改时间发生改变



将 `ll` 显示的文件列表输出到日志文件:



```
[user@ubuntu ~]$ ll > file.log
```



## 查找内容



`grep` 命令可以在指定文件中查找指定的内容



格式如下：



```
[user@ubuntu ~]$ grep 'str' file
```



比如，在 `file.log` 中，查询包含 `vim` 的内容:



```
[user@ubuntu ~]$ grep 'vim' file.log
```



常用的参数有：

- `-i` 忽略大小写 `grep -i str file`
- `-v` 反向查找（不包含） `grep -v str file`



## 管道符



管道符的主要作用就是把 `命令A` 的输出结果，交给 `命令B` 来处理，也就是 `命令A` 的执行结果，作为 `命令B` 的操作对象



格式：



```
命令A | 命令B
```



`ll` 的结果由 `more` 来处理：



```
[user@ubuntu ~]$ || /etc/ | more
```



`ll` 的结果由 `grep ` 来搜索



```
[user@ubuntu ~]$ || /etc/ |grep ssh
```



## 统计命令



`wc` 命令可以统计衣蛾问价有多少行，多少个单词，多少个字符：



```
[user@ubuntu ~]$ wc /etc/ssh/sshd_config
```



`wc-l` 只显示行数



## 下载工具



`wget` 工具可以通过下载地址，将网络资源下载本机：



```
[user@ubuntu ~]$ wget http://www.baidu.com
```



下载并重令名：



```
[user@ubuntu ~]$ wget -O a.html http://www.baidu.com
```

下载到指定目录:

```
[user@ubuntu ~]$ wget -P download http://www.baidu.com
```

断点续传:

```
[user@ubuntu ~]$ wget -c http://www.baidu.com
```

在后台下载:

```
[user@ubuntu ~]$ wget -b http://www.baidu.com
```



## curl 请求工具



`curl` 可以发起一个请求，将请求到的内容输出到终端：



```
[user@ubuntu ~]$ curl http://www.baidu.com
```



可以利用输出重定向，将本来输出到终端的内容，重定向一个文件中：



```
[user#ubuntu ~]$ curl http://www.baidu.com > c.html
```



## 查找文件或目录



### `find` 搜索命令



在系统中，搜索符合条件的文件名



**按照文件名查找 `-name` **



命令格式：



```
find 查找位置 -name 文件名
```



从 `/` 目录开始查找文件名为 `file.log` 的文件：



```
[user@ubuntu ~]$ sudo find / -name file.log
```



> 从 `/` 开始查找的话，有的目录需要管理员权限，所以加上 `sudo` 



按照文件名查找，不区分大小写：



```
[user@ubuntu ~]$ find /home/ -iname file.log
```



**按照文件大小查找 `-size` **



按照文件大小查找：



``` 
[user@ubuntu ~]$ find /home/ -size 1k
```



说明：

- 按照大小查找支持的单位有：b，c，w，k，M，G  (注意大小写)
- `1k` 文件大小等于 `1k` 的
- `+1k`  文件大小大于`1k` 的
- `-1k`  文件大小小于`1k` 的



**按照类型查找 `-type`**



说明：



- `d` 目录

- `f` 普通文件
- `l` 链接



按照文件类型查找:



```
[user@ubuntu ~]$ find . -type d	
```



搜索当前目录下所有的普通文件:



```
[user@ubuntu ~]$ find . -type f
```



### `whereis` 搜索命令



`whereis` 可以查找指定命令的二进制文件、源文件和帮助文件, 比如查看 `find` 命令:



```
[user@ubuntu ~]$ whereis find
```



如果真想查看二进制文件，可以加上 `-b` 参数：



```
[user@ubuntu ~]$ whereis -b find
```



> 这个命令不是不是特别常用, 但在有些情况下非常有用, 比如配置计划任务,需要用到命令的绝对路径的时候



### `which` 查看命令位置



`which` 查看命令所在位置：



```
[user@ubuntu ~]$ which ls
```



## 挂载



概念：Linux 所有的存储设备都必须挂载使用（Linux 中的挂载点，完全可以当做 Windows 中的盘符），区别是，Windows 的盘符是 ABCD，Linux 中的挂载点是目录



Windows 的外置存储设备比较智能，插上 U 盘，自动分配盘符，双击就能使用了，Linux 中必须手动分配盘符，也就是目录，挂载到某某目录下，才能使用。



**什么叫挂载？就是把硬件设备和空目录连接起来，就叫挂载。**



`df` 命令可以查看已经股灾的挂载点以及空间使用情况：



```
[user@ubuntu ~]$ df -h
```



> `-h` 磁盘大小显示为 `M`  ， `G` 等



### U盘挂载



**1.插入U盘**

首先将U盘插入电脑, 之后让虚拟机使用这个U盘:



**2.挂载U盘**



使用 `fdisk -l` 命令查看磁盘列表, 一般新添加的会出现在最后面, 比如新插入的U盘:

```
[user@ubuntu ~]$ sudo fdisk -l
```



> 挂载需要管理员权限



![](https://s3.ax1x.com/2020/11/12/BxLDRf.png)



> 插入的U盘的设备名字, 每个人的可能不一样



使用命令挂载U盘:



```
[user@ubuntu ~]$ sudo mount /dev/sdb1 /mnt
```



去 `/mnt` 目录检查一下, 发现里面已经有U盘的文件的



**3.卸载U盘**



当不再需要U盘的时候, 可以执行卸载命令:



```
[user@ubuntu ~]$ sudo umount /mnt
```



最后弹出U盘即可:



```
[user@ubuntu ~]$ sudo eject /dev/sdb1
```



如何检查是否挂载成功?



方式一: 执行 `df` 看一下挂载列表是否多了一个 `cdrom`

方式二: 去挂载目录, 比如本例中的 `/cdrom` 看一下, 是否有文件



## 用户



在 Linux 系统中, 用户是很重要的一环，用户管理包括用户与组账号的管理。不同的用户对不同的系统资源拥有不同的使用权限。



Linux 系统中的 root 账号通常用于系统的维护和管理, 拥有对操作系统的所有权限。



Linux 安装的过程中，系统会自动创建许多用户账号，而这些默认的用户就称为 “标准用户”。



不推荐直接使用 root 账号登录系统。



`/etc/passwd` 文件存储了用户的信息, 可以通过这个文件查看用户信息:



```
[user@ubuntu ~]$ cat /etc/passwd
```



`/etc/shadow` 影子文件储存了用户的密码，可以通过这个文件查看用户密码，密码是加密的，需要管理员权限：



```
[user@ubuntu ~]$ cat /etc/shadow
```



`/etc/group` 文件存储了用户组的信息, 可以通过这个文件查看用户组信息:



```
[user@ubuntu: ~]$ cat /etc/group
```





## 用户管理



**添加用户**



```
[user@ubuntu ~]$ useradd  (用户名)
```



- 该命令需要管理员权限
- 不能添加已存在的用户
- `-m` 参数在添加用户的时候, 同时在 `/home` 下创建用户家目录
- 在添加用户的时候,会自动添加一个同名的用户组



```
[user@ubuntu ~]$ sudo useradd -m tom
```



**设置密码**



`passwd` 命令可以给用户设置或者修改密码，如果要修改当前登录用的密码，直接使用这个命令即可：



```
[user@ubuntu ~]$ passwd
```



如果要给别的用户设置或者修改密码 ，需要管理员权限：



```
[user@ubuntu ~]$sudo passwd tom
```



**切换用户**



`su` 命令可以切换用户：



```
[user@ubuntu ~]$ su 要切换的用户名
```



切换到管理员用户需要权限，所以加上`sudo` :



```
[user@ubuntu ~]$ sudo su root
```



切换到管理员，可以省略 `root`;



```
[user@ubuntu ~]$ sudo su
```



注意: 只有具有 `sudo` 命令权限的用户, 才能使用 `sudo` 命令, 所以只有具有 `sudo`



`test` 用户是是我们安装操作系统时创建的用户, 所以 `test` 用户具有 `sudo` 命令权限, 所以可以轻易的使用 `sudo` 命令, 甚至可以通过 `sudo su` 切换到超级用户 `root`



而我们新添加的用户 `tom` 并不具备 `sudo` 命令权限, 所以不能使用 `sudo` 命令



`sudo` 权限的配置文件在 `/etc/sudoers`, 可以通过编辑这个这个文件为其他用户增加 `sudo` 权限:



```
[user@ubuntu ~]$ sudo visudo
```



> 直接使用 `sudo vim /etc/sudoers` 的话不行，这个文件是只读文件，所以通过 `visudo` 命令来修改



或者将用户加入到 `sudo` 这个组 （下面有方法）



**删除用户**



`userdel` 命令可以删除用户, `-r` 参数可以在删除用户的时候, 连同 `/home` 下的用户对应的家目录一块删除, 需要管理员权限:



```
[user@ubuntu: ~]$ sudo userdel -r tom
```



## 组管理



**查看用户所属组**



`groups` 命令可以查看用户所属组：



```
[user@ubuntu ~]$ groups
```



默认查看当前登录用户的, 如果要查看其它用户的, 后面加上用户名就行:



```
[test@ubuntu: ~]$ groups test
```



可以看到 `test` 用户在 `sudo` 用户组里面, 这也是为什么 `test` 用户可以使用 `sudo` 命令



**添加组**



`groupadd` 命令可以添加组，不能添加已存在的组，需要管理员权限：



```
[user@ubuntu: ~]$ sudo groupadd g
```



**删除组**



`groupdel` 命令可以删除组



```
[user@ubuntu: ~]$ sudo groupdel g
```





**为用户分配组**



`gpasswd` 命令可以给用户分配朱，格式如下：



````
gpasswd -a 用户名 组名
````



把 user用户加入到 `g` 组中：



```
[user@ubuntu: ~]$ sudo gpasswd -a tom g
```



**将用户从组中移除**



`gpasswd` 命令可以把用户移出分组，格式如下：



```
gpasswd -d 用户名 组名
```



把 `user` 用户从 `g` 组中移除：



```
[user@ubuntu: ~]$ sudo gpasswd -d tom g
```

