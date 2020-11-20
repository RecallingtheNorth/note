## 压缩与解压缩

### 压缩文件格式

Windows中常见的压缩文件有 `.zip` `.rar` `7z`



linux 中有 `.tar,gz` `.tar.bz2` 



### `.tar.gz` 格式



压缩成 `.tar.gz` 格式的命令是:

```
tar -zcvf 压缩后的文件名 要压缩的文件
```



比如将 `study` 目录压缩成 `study.tar.gz`:

```
tar -zcvf study.tar.gz study
```



各参数说明：

- `-z` 识别.gz格式的
- `-c` 压缩
- `-v` 显示压缩过程
- `-f` 指定压缩包名



解压缩和压缩相比，把 `-c` 参数换成 `-x` 参数即可：

```
tar -zxvf study.tar.gz study
```



`-x` 解压缩



### `.tar.bz2` 格式



如果是 `.tar.bz2` 格式，只需要把 `-z` 参数换成 `-j` 即可：



比如将 `study` 目录压缩成 `stu.tar.bz2 `:

```
tar -jcvf study.tar.bz2 study
```



- `-j` 识别 `.bz2` 



### `.zip` 格式



`linux` 系统一般不提供 `.zip` 格式的压缩，需要安装特定的程序：

```
sudo apt install zip
```



压缩成 `.zip` :

```
zip -r study.zip study
```



解压缩 `.zip` 到当前目录:

```
unzip study.zip
```



解压缩 `.zip` 到指定目录：

```
unzip -d ./tmp study.zip
```



### `.rar` 格式



`Linux` 系统一般不会提供 `.rar` 格式的压缩，需要安装特定的程序：

```
sudo apt install rar
```



压缩成 `.rar` :

```
rar a staudy.sip study
```



解压缩 `.rar` :

```
rar x study.rar
```



## 什么是计划任务



计划任务是帮助我们在特定的时间执行命令用的服务，可以达到及时备份数据的目的。



## 计划任务常用的命令



查看 `cron` 服务是否处于运行状态：

```
service cron status
```



编辑计划任务：

```
crontab -e
```



查看系统计划任务：

```
crontab -l
```



删除定时任务， `-r` 全部删除，`-i` 交互式删除：

````
ceontab -ri
````



>   注意：`crontab -r` 命令会删除所有的计划任务，如果只是要删除一个计划任务的话，可以使用 `crontab -e` 编辑计划删除，删除指定的计划任务即可



## 用户和计划任务



 因为 `Linux` 是多用户操作系统，而每个用户都可以有自己的计划任务



`crontab [-u user]` 可以操作指定用户的的计划任务，只有具备了 `-u` 权限的用户才能使用 `-u` 命令，比如 `root` 用户:

```
crontab -u test -e
```



>   不指定 `-u` 参数，操作的是当前用户的计划任务



## 编写计划任务



先写一个最简单的计划任务，每分钟输入 `hello world` 到 `hello` 文件中：

```
crontab -e
```



写入内容：

```
* * * * * /usr/bin/echo 'hello world' >> /home/user/a.log
```



保存退出，等一分钟后，查看 `/home.user/a,log` 文件，可以看到有内容写入了



## 计划任务详解



计划任务的编写格式是：

```
minute hour day month week command
```



>   各个参数 中间用空格隔开，`command` 是要执行的命令（注意：命令要写完整的路径命令）



>   使用 `whereis` 或者 `which` 加命令的方式查找命令的完整路径，如果你不知道命令的完整命令，就是用这个命令，查找命令的路径在哪



### 时间介绍

- `minute`: 一个小时中的第几分钟 `0-59`
- `hour`: 一天中第几个小时 `0-23`
- `day`: 一个月中的第几天 `1-31`
- `month`: 一年中的第几个月 `1-12`
- `week`: 一周中的星期几 `1-7`



### 符号介绍

-   星号 `*` :代表所有可能的值，例如 `month` 字段如果是星号，则表示在满足其他字段的制约条件后每月都执行该命令操作。
-   逗号 `,` :可以用逗号隔开的值指定一个列表范围，例如 `1,2,5,7,8,9`
-   中杠 `-` :可以用整数之间的中杠表示一个整数范围，例如 "2-6" 表示 `2,3,4,5,6` 
-   正斜线 `/` : 可以用正斜线指定时间的间隔频率，例如 `0-23/2` 表示每两个小时执行一次。同时正斜线可以和星号一起使用，例如 `*/10` ,如果用在 `minute` 字段，表示每十分钟执行一次



### 一些例子

`31号5点10分` 来执行一条命令:

```
10 5 31 * * command
```



一年中 `3,5,6,10` 这四个月份 `3点5分` 执行命令:

```
5 3 * 3,5,6,10 * command
```



>   删除计划任务，但注意，是全部删除了，如果想删除一条，vim进去，dd删除你想删除的即可。



### 一些注意

- 选项不能为空，必须填写，不知道的内容用*代表，表示任意时间
- 每个时间字段都是可以指定多个值，不连续的用(,)隔开，连续的值(-)减号隔开
- 间隔固定的时间执行书写格式`*/n` 格式`n`代表要间隔多少分钟，多少小时，多少天等
- 星期几和第几天不能同时出现
- 最小的时间范围是分钟，最大的时间范围是月



### 如何每秒执行一次



思路是，可以让计划任务每分钟执行一个脚本文件，在这个脚本文件中每秒训话一次，循环60秒



编写脚本：

```
vim crontab.sh
```



写入内容：

```
# !/bin.bash

for((i = 0; i < 60;i ++))do
	$(/bin/echo 'aaa' >> /tmp/aaa.log)
	sleep 1
done

exit 0
```



给脚本文件可执行权限：

```
chmod 755 crontab.sh
```



编写计划任务：

```
crontab -e
```



写入：

```
* * * * * /home/user/crontab.sh
```



这样就可以每秒执行一次了



最后删除计划任务：

```
crontab -r
```



## 如何安装软件



软件包的安装可以分为三大类：



1.  二进制形式的，比如说 `rpm` , `rpm` 包有平台的限制

2.  源代码编译安装，源代码可以使用最新的版本，并且可以使用最新的功能，可以定制，让她更符合自身的习惯和需求。并且源代码一旦在使用的时候可以在不同的平台上实现。

3.  包管理工具，不同操作系统提供各自的包管理工具，比如 `CentOS` 的 `yum` ,`Ubuntu` 的 `apt` 等

    

## `rpm` 安装

 	`rpm` 一般多用于 `Fedora` 和 `RHEL` 平台, 我们这里学习的是 `Ubuntu`, 属于 `Debian` 平台, 一般不会使用 `rpm` 来管理软件, 这里就不多做介绍了



## 编译安装



编译安装的优点:

- 可以获得最新的软件，及时修复bug
- 根据用户的需求，灵活定制软件功能

编译安装一般会经历这样几个步骤:

1. 解压源代码
2. 由 `configure` 针对当前系统、软件环境，配置好安装参数
3. 使用 `make` 将源代码编译为二进制的可执行程序
4. 使用 `make install` 安装程序



 这里仅作为了解, 暂不细讲



## 包管理工具

编译安装比较麻烦，如果不是有特殊的需要，一般使用包管理工具进行软件管理即可。



`Ubuntu` 使用的包管理工具是 `apt`, 通过这个命令, 我们可以进行软件的管理, 包括安装卸载等



> `Ubuntu 16.04` 之前, 使用的是 `apt-get` 命令, `Ubuntu 16.04` 之后, 可以使用 `apt` 或者 `apt-get`



​	apt 命令是一个功能强大的命令行工具，它不仅可以更新软件包列表索引、执行安装新软件包、升级现有软件包，还能够升级整个 Ubuntu 系统(apt 是 Debian 系操作系统的包管理工具)。



### 刷新存储库索引（更新源）



`apt` 根据 `/etc/apt/sources.list` 里的软件源地址列表搜索目标软件、并通过维护本地软件包来安装和卸载软件。



所以，在安装软件之前，先执行 `apt update` 命令，刷新一下储存库索引：

```
sudo apt update
```



>   一般使用 `apt` 都需要管理员权限，所以加上 `sudo`



### 搜索软件



如果不清楚我们要安装的软件是否在软件源里面，可以使用搜索命令：

```
sudo apt search wget
```



如果找到软件，就会给我们显示一个列表，如果我们安装过某个软件，会有 `installed` 的显示：

![](https://s3.ax1x.com/2020/11/16/DkTrLV.png)



### 安装软件



使用 `apt install` 可以安装软件，比如安装 `cmatrix` : 

```
sudo apt install cmatrix
```



安装完成后，执行一下看看效果：

```
cmatrix
```



![](https://s3.ax1x.com/2020/11/16/Dk7nmV.png)



-   按 `q` 退出

### 其他常用命令



**卸载软件**

```
sudo apt remove 软件名
```



**升级所有可以升级的软件**

```
sudo apt upgrade
```



### `apt` 与 `apt-get` 的对比

|     apt 命令     |      取代的命令      |           命令的功能           |
| :--------------: | :------------------: | :----------------------------: |
|   apt install    |   apt-get install    |           安装软件包           |
|    apt remove    |    apt-get remove    |           移除软件包           |
|    apt purge     |    apt-get purge     |      移除软件包及配置文件      |
|    apt update    |    apt-get update    |         刷新存储库索引         |
|   apt upgrade    |   apt-get upgrade    |     升级所有可升级的软件包     |
|  apt autoremove  |  apt-get autoremove  |       自动删除不需要的包       |
| apt full-upgrade | apt-get dist-upgrade | 在升级软件包时自动处理依赖关系 |
|    apt search    |   apt-cache search   |          搜索应用程序          |
|     apt show     |    apt-cache show    |          显示安装细节          |





## LNMP介绍



`LNMP`  是指一组通常一起使用来运行动态网站或者服务器的自由软件名称首字母缩写。L指Linux，N指Nginx，M一般指MySQL，也可以指MariaDB，P一般指PHP，也可以指Perl或Python。



`LNMP` 代表的就是：Linux系统下Nginx+MySQL+PHP这种网站服务器架构。

国外喜欢简称为 `LEMP`，搜英文资料需要搜 `LEMP`



Linux` 是一类Unix计算机操作系统的统称，是目前最流行的免费操作系统。代表版本有：debian、centos、ubuntu、fedora、gentoo等。



`Nginx` 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP代理服务器。



`Mysql` 是一个小型关系型数据库管理系统。



`PHP` 是一种在服务器端执行的嵌入HTML文档的脚本语言。



 这四种软件均为免费开源软件，组合到一起，成为一个免费、高效、扩展性强的网站服务系统。



## 安装Nginx



```
sudo apt isntall nginx
```



Nginx 安装之后, 会自动启动, 并监听 80 端口, 开启一个Web服务



在宿主机(本机)中,输入你虚拟机的ip, 就可以访问Nginx这个Web服务:





## 安装Mysql



```
sudo apt install mysql-server
```



查看是否安装成功：

```
mysql --version
```



![sudo cat /etc/mysql/debian.cnf](https://s3.ax1x.com/2020/11/16/DkOHTP.png)



**关于mysql用户和密码**



在 `mysql  5.7` 以前, 在安装mysql的过程中, 会要求设置用户名和密码, 在 `mysql  5.7` 之后, 就不用设置用户名和密码了, 而是给我们生成了一个默认的用户和密码,  记录在`debian.cnf `:

```
sudo cat /etc/mysql/debian.cnf
```



## 安装PHP



```
sudo apt install php
```



>   可以安装指定版本的php，如果不指定，默认安装最新版本的PHP



查看安装是否成功：



```
php -v
```



![](https://s3.ax1x.com/2020/11/16/DkXycQ.png)



## 建立nginx 与 php 的通信



`Nginx` 与 `php` 通信，实际上是与 `php-fpm` 需要安装对应php版本的 `php-fpm`

```
sudo apt install php7.4-fpm
```



有两种通信方式：



一个是通过 `php-fpm` 使用 ` unix sockets` 套接字协议进行通信

一个是通过 `php-cgi` 使用 `tcp sockets` 协议通信

这里我们使用 `php-fpm`



修改 `nginx` 的站点配置文件：

```
sudo vim /etc/nginx/sites-available/default
```



在44行添加`index.php` :

```
index index.php index.html index.htm index.ngin x-debian.html;
```



在56-63行的配置如下：

```
 location ~ \.php$ {
            include snippets/fastcgi-php.conf;

    #       # With php-fpm (or other unix sockets):
            fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
    #       # With php-cgi (or other tcp sockets):
    #       fastcgi_pass 127.0.0.1:9000;
    }
```



检查 `nginx` 的配置是否正常：

````
sudo nginx -t 
````



平滑重启 `nginx` :

```
sudo nginx -s reload
```



在 `/var/www/html` 目录下新加一个 `info.php`文件，写入 ：

```
<?php
	phpinfo()
```



在宿主机浏览器中访问这个文件：

![](https://s3.ax1x.com/2020/11/16/Dkjfrd.png)





## 关于 `php-fpm` 的配置



`php-fpm` 的配置文件是在 `/etc/php/7.4/fpm/pool.d/www.conf`, 如果想要把 `unix sockets` 模式改成 `php-cgi` 模式, 编辑这个配置文件:

```
sudo vim /etc/php/7.4/fpm/pool.d/www.conf
```

找到：

```
listen = /run/php/php7.4-fpm.sock
```

改成：

```
listen = 127.0.0.1:9000
```



最后重启 `php-fpm` :

```
sudo service php7.4-fpm restart
```



在 `/etc/php/7.4/fpm/pool.d/www.conf` 配置文件中, 有很多与性能有关的优化, 比如

```
pm.max_children：静态方式下开启的php-fpm进程数量。
pm.start_servers：动态方式下的起始php-fpm进程数量。
pm.min_spare_servers：动态方式下的最小php-fpm进程数量。
pm.max_spare_servers：动态方式下的最大php-fpm进程数量。
```



静态模式(static)/动态模式(dynamic)的设置:

```
pm=dynamic
```