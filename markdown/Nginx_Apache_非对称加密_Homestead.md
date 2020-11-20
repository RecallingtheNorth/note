## Nginx介绍

Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。

Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，在BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。


## 在 `Ubuntu` 中的 `Nginx` 目录结构和说明

其他系统目录结构可能不一样,但是配置文件都是一样通用的,这里简单说一下ubuntu的目录结构

如果是使用 `apt` 安装的 `nginx` ,配置文件目录在:

```
/etc/nginx/
```

`cd` 到这个目录之后,`ls`查看所有文件:

```
nginx.conf  
# 这个是nginx的主配置文件,里面包含了当前目录的所有配置文件,
# 只不过有的是注释状态,需要的时候自行开启(后面几个常用的)

conf.d
# 这是一个目录,里面可以写我们自己自定义的配置文件,文件结尾一
# 定是.conf才可以生效(当然也可以通过修改nginx.conf来取消这个限制)

sites-enabled
# 这里面的配置文件其实就是sites-available里面的配置文件的软
# 连接,但是由于nginx.conf默认包含的是这个文件夹,所以我们在
# sites-available里面建立了新的站点之后,还要建立个软连接到sites-enabled里面才行

sites-available
# 这里是我们的虚拟主机的目录，我们在在这里面可以创建多个虚拟主机
```

## 多站点配置

为什么要配置多站点:

当我们有了一个服务器之后,为了不浪费服务器的资源,我们可以在一个
服务器上放置多个网站项目,它们共同使用80端口,通过不同的 `servername`
来区分不同的网站项目,在实际上线的项目中,这个 `servername` 就是我们的域名啦

具体配置(我们只举例一个,多个重复操作就行):

默认已经有一个站点了,就是 `defalt`,在 `sites-available` 里面有
一个 `default` 文件,就是默认站点的配置, `servername` 是 `localhost`
不建议直接修改这个默认站点,我们可以复制一个:

```
[test@ubuntu: ~]$ cd /etc/nginx/sites-available/
[test@ubuntu: ~]$ sudo cp default web1.com
```

别忘了建立个软连接,不然新站点不会生效滴:

```
[test@ubuntu: ~]$ sudo ln -s /etc/ngix/sites-available/web1.com /etc/nginx/sites-enabled/web1.com
```

现在就开始修改我们的新站点配置:

```	
[test@ubuntu: ~]$ sudo vim web1.com
```

找到21行的这句配置(:set nu可以显示行号):

```
listen 80 default_server;
```

改成:

```
listen 80; //注意:default_server是设置默认站点的,我们新建立的站点不需要
```

找到24行:

```
root /usr/share/nginx/html
```

改成:

```
root /your server path  (写你自己的网站目录)
```

检查配置是否正确:

```
[test@ubuntu: ~]$ nginx -t
```

重启nginx服务:

```
[test@ubuntu: ~]$ sudo /etc/init.d/nginx restart
```

如果不想影响正在运行的项目, 可以平滑重启:

```
[test@ubuntu: ~]$ sudo nginx -s reload
```

OK,到这里一个新的站点已经配置完成了,本机测试:

`windows` 修改hosts文件:

文件在:

```
C:\Windows\System32\drivers\etc\hosts
```

会出现权限问题不让修改,自行百度怎么修改这个文件的权限

`mac os` 修改hosts文件:

```
sudo /etc/hosts/
```

在文件中加入:

```
# nginx服务器的ip 新站点的域名
192.168.1.222 web1.com
```

浏览器访问web1.com测试结果

全部配置完成,多个站点的话,多重复操作几次就行啦!

**为什么要使用软连接?而不是直接使用配置**

是因为我们如果想要停止这个网站的时候, 直接删除掉软连接里面的文件就可以了, 不用删除原始配置文件

如果想要再启动网站的时候, 只要再次建立一个新的软连接就可以了

## 反向代理

**正向代理:**

正向代理 是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。

客户端必须要进行一些特别的设置才能使用正向代理。

主要为了越过局域网内的防火墙实现访问网站

**反向代理:**

反向代理正好相反，对于客户端而言它就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理的命名空间(name-space)中的内容发送普通请求，接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端，就像这些内容原本就是它自己的一样。

为了将防火墙后面的服务器提供给Internet用户访问

也可以实现负载均衡,动静分离,url策略

具体配置参考文章结尾附录

### 使用反向代理配置Node项目

安装 `nodejs`:

```
[test@ubuntu: ~]$ sudo apt install nodejs
```

在 `/var/www` 目录下创建 `node` 目录:

```
[test@ubuntu: ~]$ cd /var/www
[test@ubuntu: ~]$ mkdir node
```

创建 `app.js` 写入代码:

```
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(3000);

console.log('Server running at http://127.0.0.1:3000/');

```

在后台运行 `app.js`:

```
[test@ubuntu: ~]$ node app.js &
```

浏览器中访问 `192.168.56.110:3000` 可以正常访问

但是总不能让用户访问网站还要带上 `3000` 端口吧? 这时候, 配置 `Nginx` 反向代理 可以解决这个问题

复制一份 `/etc/nginx/sites-enabled` 中的配置为 `node.test`, 写入下面配置:

```
server {
        listen 80;
        listen [::]:80;

        server_name node.test;

        location / {
                proxy_pass http://localhost:3000;
                proxy_set_header   Host    $host;
                proxy_set_header   X-Real-IP   $remote_addr;
                proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;

        }
}
```

建立软连接, 使配置生效:

```
[test@ubuntu: ~]$ sudo ln -s /etc/nginx/sites-available/node.test /etc/nginx/sites-enabled/
```

重启 `nginx`:

```
[test@ubuntu: ~]$ sudo nginx -s reload
```

在宿主机的 `hosts` 文件中加入域名解析:

```
192.168.56.110 node.test
```

浏览器中访问 `node.test`

## 负载均衡

前期准备:
	
- nginx服务器 192.168.1.100
- web服务器1  192.168.1.101
- web服务器2  192.168.1.102

修改配置文件:

```
[test@ubuntu: ~]$ sudo vim /etc/nginx/sites-available/default
```

这样配置:

```
upstream a.com {
  server  192.168.1.101:80;  #有多少个服务器就添加多少个ip
  server  192.168.1.102:80;
}

server{
    listen 80;
    server_name a.com;
    location / {
        proxy_pass         http://a.com;   #这个地址一定是上面定义的负载均衡的名字
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}
```

`nginx`主服务器也可以当做一个负载均衡服务器,但是由于80端口已经给负载均衡食用了,所有如果我们还使用80端口的话,就会造成一个死循环,我们可以再给主服务器添加一个服务器,并使用不同的8080端口,这样,主服务器也可以当做一个负载均衡的子服务器了,不会造成资源的浪费:

```
server{
    listen 8080;
    server_name a.com;
    index index.html;
    root /data0/htdocs/www;
}

upstream a.com {
  server  192.168.5.126:80;
  server  192.168.5.27:80;
  server  127.0.0.1:8080;
}
```

重启nginx服务器就可以查看效果了:

```
[test@ubuntu: ~]$ nginx -s reload
````

**扩展:**

1. 轮询(默认方式)

每个请求按时间顺序逐一分配到后端服务器,如果后端服务器down掉,能自动剔除

2. weight

指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。

```
upstream bakend {
	server 192.168.159.10 weight=10;
	server 192.168.159.11 weight=10;
}
```

3. ip_hash


每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

```
upstream resinserver{
	ip_hash;
	server 192.168.159.10:8080;
	server 192.168.159.11:8080;
}
```

4. fair(第三方)

按后端服务器的响应时间来分配请求，响应时间短的优先分配。

```
upstream resinserver{
	server server1;
	server server2;
	fair;
}
```

5. url_hash（第三方

例：在 `upstream` 中加入 `hash` 语句，`server` 语句中不能写入 `weight` 等其他的参数，`hash_method` 是使用的hash算法

```
upstream resinserver{
	server squid1:3128;
	server squid2:3128;
	hash $request_uri;
	hash_method crc32;
}
```

注意:

```
# 定义负载均衡设备的Ip及设备状态
upstream resinserver{
	ip_hash;
	server 127.0.0.1:8000 down;
	server 127.0.0.1:8080 weight=2;
	server 127.0.0.1:6801;
	server 127.0.0.1:6802 backup;
}
```

在需要使用负载均衡的server中增加

```
proxy_pass http://resinserver/;
```

每个设备的状态设置为:

1. down 表示单前的 `server` 暂时不参与负载
2. weight 默认为1 `weight` 越大，负载的权重就越大。
3. max_fails ：允许请求失败的次数默认为1.当超过最大次数时，返回 `proxy_next_upstream` 模块定义的错误, `fail_timeout:max_fails` 次失败后，暂停的时间。
5. backup： 其它所有的非 `backup` 机器 `down` 或者忙的时候，请求 `backup` 机器。所以这台机器压力会最轻。
	

nginx支持同时设置多组的负载均衡，用来给不用的server来使用。

```
client_body_in_file_only # 设置为On 可以将client post过来的数据记录到文件中用来做debug

client_body_temp_path # 设置记录文件的目录 可以设置最多3层目录

location # 对URL进行匹配.可以进行重定向或者进行新的代理 负载均衡
```

## 动静分离

动静分离也是利用负载均衡的原理来实现的,为了便于管理,我们把ip分配的配置
写在conf.d这个文件夹里面:

```
[test@ubuntu: ~]$ cd conf.d

[test@ubuntu: ~]$ sudo vim upstream.conf
```

里面写上动静分离的分配(以PHP和静态文件为例子):

```
upstream php {
	server 192.168.10.10:80  # php给这个服务器处理
}

upstream static {
	server 192.168.10.11:80 # html给这个服务器处理
}
```

然后在server服务器里面这样配置:

```
server{
    listen 80;
    server_name a.com;
    location / { #匹配所有静态资源文件用这个代理
        proxy_pass         http://static;   #这个地址一定是上面定义的负载均衡的名字
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    }

	location ~ \.php$ { #匹配php文件用这个代理
        proxy_pass         http://php;   #这个地址一定是上面定义的负载均衡的名字
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}
```

## Nginx 完整配置文件说明

`nginx.conf` 文件配置详细说明

```
user  www www; #nginx  系统用户和系统用户组
  
   worker_processes auto; # 启动进程
  
   error_log  /home/wwwlogs/nginx_error.log  crit; # 错误日志 
  
   pid        /usr/local/nginx/logs/nginx.pid;#主程序的pid 的保存文件
  
   #Specifies the value for maximum file descriptors that can be opened by this     process.
  worker_rlimit_nofile 51200;#文件描述符数量
 
  events
      {
          use epoll;#nging建议使用epoll
          worker_connections 51200;#单个工作进程最大允许连接数
          multi_accept on;#设置一个进程是否同时连接多个网络连接
      }
 
  http #主要配置选项
      {
          include       mime.types;#文件扩展名与文件类型映射
          default_type  application/octet-stream;#默认文件类型 
 
          server_names_hash_bucket_size 128;#设置请求缓存
          client_header_buffer_size 32k;#设置请求缓存
          large_client_header_buffers 4 32k;#设置请求缓存
          client_max_body_size 50m;#设置请求缓存
 
          sendfile   on;#开启高效传输默认
          tcp_nopush on;# 激活tcp_nopush参数，可以允许把header和文件的开始放在一个文件里面发布减少网络报文段的数量
 
          keepalive_timeout 60;#连接超时时间
 
          tcp_nodelay on;#禁止nodelay算法，也就是不缓存数据    
 		    #fastcgi 相关参数，为了改善网站性能，减少资源占用，提高访问速度
          fastcgi_connect_timeout 300;
          fastcgi_send_timeout 300;
          fastcgi_read_timeout 300;
          fastcgi_buffer_size 64k;
          fastcgi_buffers 4 64k;
          fastcgi_busy_buffers_size 128k;
          fastcgi_temp_file_write_size 256k;
 			#网络压缩
          gzip on;
          gzip_min_length  1k;
          gzip_buffers     416k;
          gzip_http_version 1.1;
          gzip_comp_level 2;
          gzip_types     text/plain application/javascript application/x-javas    cript text/javascript text/css application/xml application/xml+rss;
          gzip_vary on;
          gzip_proxied   expired no-cache no-store private auth;
          gzip_disable   "MSIE [1-6]\.";
 
          #limit_conn_zone $binary_remote_addr zone=perip:10m;
          ##If enable limit_conn_zone,add "limit_conn perip 10;" to server sec    tion.
 			#隐藏响应的header和错误通知中的版本号
          server_tokens off;
          #log format
		   #设置日志模式
          log_format  access  '$remote_addr - $remote_user [$time_local] "$req    uest" '
               '$status $body_bytes_sent "$http_referer" '
               '"$http_user_agent" $http_x_forwarded_for';
                  access_log off;
 
  server #主页目录文件，这是我们主要设置的目录
      {
          listen 80 default_server;#监听端口
          #listen [::]:80 default_server ipv6only=on;
          server_name www.lnmp.org;#服务器名称
          index index.html index.htm index.php;#默认网站页面
          root  /home/wwwroot/default;#网站主目录
 
          #error_page   404   /404.html;
          include enable-php.conf;
 			#开启静态的status状态
          location /nginx_status
          {
              stub_status on;
              access_log   off;
          }
 			#处理静态文件保存的时间30天
          location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
          {
              expires      30d;
          }
 			#js和css文件处理的保存时间12小时
          location ~ .*\.(js|css)?$
          {
              expires      12h;
         }
 
          location ~ /\.
          {
              deny all;
          }
 
          access_log  /home/wwwlogs/access.log  access;#正确的访问日志
      }
  include vhost/*.conf; # 配置子配置文件生效
  }
```

## Apache 介绍

Apache(音译为阿帕奇)是世界使用排名第一的Web服务器软件。它可以运行在几乎所有广泛使用的计算机平台上，由于其跨平台和安全性被广泛使用，是最流行的Web服务器端软件之一。它快速、可靠并且可通过简单的API扩充，将Perl/Python等解释器编译到服务器中。

本来它只用于小型或试验Internet网络，后来逐步扩充到各种Unix系统中，尤其对Linux的支持相当完美。Apache有多种产品，可以支持SSL技术，支持多个虚拟主机。Apache是以进程为基础的结构，进程要比线程消耗更多的系统开支，不太适合于多处理器环境，因此，在一个Apache Web站点扩容时，通常是增加服务器或扩充群集节点而不是增加处理器。

## 安装Apache

在Ubuntu上安装apache是比较简单的, 只需要一条命令就行:

```
[test@ubuntu: ~]$ sudo apt install apache2
```

可以通过键入以下命令来验证Apache是否正在运行：

```
[test@ubuntu: ~]$ sudo systemctl status apache2
```

或者:

```
[test@ubuntu: ~]$ service apache2 status
```

如果没有正在运行, 手动启动:

```
[test@ubuntu: ~]$ sudo systemctl start apache2
```

如果不能正常启动, 可能是有其他web服务器占用了 80 端口, 检查端口是否被占用:

```
[test@ubuntu: ~]$ sudo netstat -tunlp | grep 80
```

或者使用:

```
[test@ubuntu: ~]$ sudo lsof:80
```

如果看到80端口被 `nginx` 所占用的话, 可以先关闭 `nginx`, 再启用 `apache`

> 另外, 如果开启了防火墙, 记得设置 `apache` 为允许

## Ubuntu下的apache目录结构

在 Ubuntu 系统里面, apache的目录结构和nginx有点类似:

```
├── apache2.conf
├── conf-available
│   ├── php7.4-fpm.conf
│   └── serve-cgi-bin.conf
├── conf-enabled
│   ├── charset.conf -> ../conf-available/charset.conf
├── envvars
├── magic
├── mods-available
│   ├── access_compat.load
│   └── xml2enc.load
├── mods-enabled
│   ├── access_compat.load -> ../mods-available/access_compat.load
├── ports.conf
├── sites-available
│   ├── 000-default.conf
│   └── default-ssl.conf
└── sites-enabled
    └── 000-default.conf -> ../sites-available/000-default.conf
```

`apache2.conf` 是 apache2 的主配置文件, 通过查看这个文件, 可以看到:

```
221 # Include generic snippets of statements
222 IncludeOptional conf-enabled/*.conf
223 
224 # Include the virtual host configurations:
225 IncludeOptional sites-enabled/*.conf
```

它包含了 `onf-enabled/*.conf` 和 `sites-enabled/*.conf`, 所以真正生效的配置是这两个目录,这个两个目录是其他对应的两个目录的软连接

**为什么要使用软连接?而不是直接使用配置**

是因为我们如果想要停止这个网站的时候, 直接删除掉软连接里面的文件就可以了, 不用删除原始配置文件

如果想要再启动网站的时候, 只要再次建立一个新的软连接就可以了

## 虚拟主机(多站点)配置

同样的, 为了最大化利用服务器的资源, apache也是支持多站点的配置

虚拟主机的配置文件是 `sites-available/000-default.conf`, 复制一个这个文件:

```
[test@ubuntu: ~]$ sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/a.com.conf
```

> 注意: 虚拟主机一定要以 .conf 结尾, 因为主配置文件中包含的是  *.conf

编辑 `/etc/apache2/sites-available/a.com.conf`:

```
[test@ubuntu: ~]$ sudo vim /etc/apache2/sites-available/a.com.conf
```

修改成配置如下:

```
<VirtualHost *:80>
    #主站点名称（网站的主机名）
    ServerName a.com 
    
    #主站点名称的别名
    ServerAlias www.a.com 
    
    #管理员的邮件地址
    ServerAdmin webmaster@a.com 
    
    #主站点的网页存储位置
    DocumentRoot /var/www/a 

    # 目录进行访问控制
    <Directory /var/www/a> 
        # Indexex 关闭目录索引; FollowSymLinks 允许文件系统使用符号连接
        Options -Indexes +FollowSymLinks 
        
        #支持.htaccess
        AllowOverride All 
    </Directory>

    # 错误日志
    ErrorLog ${APACHE_LOG_DIR}/a.com-error.log 
    
    # 访问日志
    CustomLog ${APACHE_LOG_DIR}/a.com-access.log combined 
</VirtualHost>
```

启用虚拟主机的最简单方法是使用 a2ensite 帮助程序：

```
[test@ubuntu: ~]$ sudo a2ensite a.com
```

或者手动建立软连接文件, 不然配置不会生效:

```
[test@ubuntu: ~]$ sudo ln -s /etc/apache2/sites-available/a.com.conf /etc/apache2/sites-enabled/
```

重启 `apache`:

```
[test@ubuntu: ~]$ sudo systemctl restart apache2
```

创建项目目录, 并写入 `index.html`:

```
[test@ubuntu: ~]$ sudo mkdir /var/www/a
[test@ubuntu: ~]$ sudo vim /var/www/a/index.html
```

写入:

```
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Welcome to a.com</title>
  </head>
  <body>
    <h1>Success! a.com home page!</h1>
  </body>
</html>
```

配置宿主机的 `hosts` 文件:

```
192.168.56.110 a.com
```

浏览器中访问即可

如果要停用网站, 只需要删除 `/etc/apache2/sites-enabled/` 里面的软连接, 重启 `apache` 即可

## 公钥与私钥介绍

### 1、公钥密码体制的核心思想是

加密和解密采用不同的密钥。这是公钥密码体制和传统的对称密码体制最大的区别。对于传统对称密码而言，密文的安全性完全依赖于 密钥的保密性，一旦密钥泄漏，将毫无保密性可言。但是公钥密码体制彻底改变了这一状况。在公钥密码体制中，公钥是公开的，只有私钥是需要保密的。知道公钥 和密码算法要推测出私钥在计算上是不可行的。这样，只要私钥是安全的，那么加密就是可信的。

显然，对称密码和公钥密码都需要保证密钥的安全，不同之处在于密钥的管理和分发上面。在对称密码中，必须要有一种可靠的手段将加密密钥（同时也是解密 钥）告诉给解密方；而在公钥密码体制中，这是不需要的。解密方只需要保证自己的私钥的保密性即可，对于公钥，无论是对加密方而言还是对密码分析者而言都是 公开的，故无需考虑采用可靠的通道进行密码分发。这使得密钥管理和密钥分发的难度大大降低了。

### 2、加密和解密

发送方利用接收方的公钥对要发送的明文进行加密,接受方利用自己的私钥进行解密,其中公钥和私钥匙相对的,任何一个作为公钥,则另一个就为私钥.但是因为非对称加密技术的速度比较慢,所以,一般采用对称加密技术加密明文,然后用非对称加密技术加密对称密钥,即数字信封技术.签名和验证:发送方用特殊的hash算法，由明文中产生固定长度的摘要，然后利用自己的私钥对形成的摘要进行加密，这个过程就叫签名。接受方利用发送方的公钥解密被加密的摘要得到结果A，然后对明文也进行hash操作产生摘要B.最后,把A和B作比较。此方式既可以保证发送方的身份不可抵赖，又可以保证数据在传输过程中不会被篡改。

### 3、要分清它们的概念

**加密和认证:**

首先我们需要区分加密和认证这两个基本概念。
加密是将数据资料加密，使得非法用户即使取得加密过的资料，也无法获取正确的资料内容， 所以数据加密可以保护数据，防止监听攻击。其重点在于数据的安全性。身份认证是用来判断某个身份的真实性，确认身份后，系统才可以依不同的身份给予不同的 权限。其重点在于用户的真实性。两者的侧重点是不同的。

**公钥和私钥:**

其次我们还要了解公钥和私钥的概念和作用。
在现代密码体制中加密和解密是采用不同的密钥（公开密钥），也就是非对称密钥密码系统，每个通信方均需要两个密钥，即公钥和私钥，这两把密钥可以互为加解密。公钥是公开的，不需要保密，而私钥是由个人自己持有，并且必须妥善保管和注意保密。

**公钥私钥的原则:**

一个公钥对应一个私钥。
密钥对中，让大家都知道的是公钥，不告诉大家，只有自己知道的，是私钥。如果用其中一个密钥加密数据，则只有对应的那个密钥才可以解密。如果用其中一个密钥可以进行解密数据，则该数据必然是对应的那个密钥进行的加密。非对称密钥密码的主要应用就是公钥加密和公钥认证，而公钥加密的过程和公钥认证的过程是不一样的，下面我就详细讲解一下两者的区别。

## 数字签名

1.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080901.png)

鲍勃有两把钥匙，一把是公钥，另一把是私钥。

2.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080902.png)

鲍勃把公钥送给他的朋友们----帕蒂、道格、苏珊----每人一把。

3.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080903.png)

苏珊要给鲍勃写一封保密的信。她写完后用鲍勃的公钥加密，就可以达到保密的效果。

4.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080904.png)

鲍勃收信后，用私钥解密，就看到了信件内容。这里要强调的是，只要鲍勃的私钥不泄露，这封信就是安全的，即使落在别人手里，也无法解密。

5.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080905.png)

鲍勃给苏珊回信，决定采用"数字签名"。他写完后先用Hash函数，生成信件的摘要（digest）。

6.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080906.png)

然后，鲍勃使用私钥，对这个摘要加密，生成"数字签名"（signature）。

7.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080907.png)

鲍勃将这个签名，附在信件下面，一起发给苏珊。

8.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080908.png)

苏珊收信后，取下数字签名，用鲍勃的公钥解密，得到信件的摘要。由此证明，这封信确实是鲍勃发出的。

9.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080909.png)

苏珊再对信件本身使用Hash函数，将得到的结果，与上一步得到的摘要进行对比。如果两者一致，就证明这封信未被修改过。

10.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080910.png)

复杂的情况出现了。道格想欺骗苏珊，他偷偷使用了苏珊的电脑，用自己的公钥换走了鲍勃的公钥。此时，苏珊实际拥有的是道格的公钥，但是还以为这是鲍勃的公钥。因此，道格就可以冒充鲍勃，用自己的私钥做成"数字签名"，写信给苏珊，让苏珊用假的鲍勃公钥进行解密。

11.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080911.png)

后来，苏珊感觉不对劲，发现自己无法确定公钥是否真的属于鲍勃。她想到了一个办法，要求鲍勃去找"证书中心"（certificate authority，简称CA），为公钥做认证。证书中心用自己的私钥，对鲍勃的公钥和一些相关信息一起加密，生成"数字证书"（Digital Certificate）。

12.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080912.png)

鲍勃拿到数字证书以后，就可以放心了。以后再给苏珊写信，只要在签名的同时，再附上数字证书就行了。

13.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080913.png)

苏珊收信后，用CA的公钥解开数字证书，就可以拿到鲍勃真实的公钥了，然后就能证明"数字签名"是否真的是鲍勃签的。

14.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080914.jpg)

下面，我们看一个应用"数字证书"的实例：https协议。这个协议主要用于网页加密。

15.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080915.png)

首先，客户端向服务器发出加密请求。

16.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080916.png)

服务器用自己的私钥加密网页以后，连同本身的数字证书，一起发送给客户端。

17.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080917.png)

客户端（浏览器）的"证书管理器"，有"受信任的根证书颁发机构"列表。客户端会根据这张列表，查看解开数字证书的公钥是否在列表之内。

18.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080918.png)

如果数字证书记载的网址，与你正在浏览的网址不一致，就说明这张证书可能被冒用，浏览器会发出警告。

19.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080919.jpg)

如果这张数字证书不是由受信任的机构颁发的，浏览器会发出另一种警告。

20.

![](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011080920.png)

如果数字证书是可靠的，客户端就可以使用证书中的服务器公钥，对信息进行加密，然后与服务器交换加密信息。

## 如何生成

打开 `cmd` 使用 `ssh` 工具生成, 输入回车:

```
ssh-keygen
```

之后一直回车, 直到结束

会在当前用户的家目录下的 `.ssh` 目录里面生成一对公钥和私钥, 公钥可以放到任意需要认证的地方, 而私钥一定保存在本机,不能外传和丢失, 如果私钥泄露, 建议更换私钥和公钥

![](http://cdn.wkbook.cn/img/20201117001123.png)

使用工具生成, 比如使用 `puttygen`, 在安装 `putty` 的时候, 会自动安装 `puttygen`, 在开始菜单, 或者搜索中, 找到这个程序, 运行程序, 点击按钮进行生成:

![](http://cdn.wkbook.cn/img/20201117001515.png)

可以分别保存公钥和私钥:

![](http://cdn.wkbook.cn/img/20201117001752.png)

## 实际应用

**远程连接服务器**

要使用私钥远程连接服务器, 需要在服务器的用户的家目录中的 `.ssh/authorized_keys` 文件中写入公钥, 如果没有这个目录或者文件, 可以创建:

```
[test@ubuntu: ~]$ mkdir .ssh
[test@ubuntu: ~]$ touch .ssh/authorized_keys
```

> 注意: 你要使用私钥登录哪个用户, 就要在哪个用户的家目录的 `.ssh/authorized_keys` 写入你的公钥

要把私钥放到宿主机的家目录里面的 `.ssh` 目录才可以, 并且名字是 `id_rsa`

在宿主机打开cmd命令, 使用 `ssh` 工具进行连接:

```
ssh -i C:\Users\m1339\.ssh\id_rsa test@192.168.56.110
```

> 注意: 公钥你放到了哪个用户家目录,就用哪个用户登录

**Git中使用**

在使用Git的时候, 需要进入认证, 也可以使用公钥和私钥, 这个在学习Git的时候, 会详细讲解

## 介绍

Laravel 致力于让整个 PHP 开发体验变得更愉快，这其中也包括你的本地开发环境。Vagrant 提供了一种简单、优雅的方式来管理和配置虚拟机。

Laravel Homestead 是 Laravel 官方预封装的 Vagrant Box，它为你提供了一个完美的开发环境，让你不需要再本地开发机器上安装 PHP、Web 服务器以及其他的服务器软件。你再也不用担心会弄乱你的操作系统了！Vagrant Box 完全是一次性的。如果出现问题，你可以在几分钟内删除并重新创建 Box！

Homestead 可以在任何 Windows、Mac 或 Linux 系统上运行，它预装好了 Nginx、PHP、MySQL、PostgreSQL、Redis、Memcached、Node 以及开发令人惊叹的 Laravel 应用程序所需的所有其他软件。

> Vagrant 最主要的作用就是文件共享, 将宿主机的代码, 自动同步到虚拟机中去

> homestead 实际就是一个Ubuntu的系统, 里面安装了我们开发所需要的所有软件

## 安装

**安装前的准备**

要先安装  `VirtualBox 6.x`(在学习Linux的时候, 一般我们都安装了) 或者其他虚拟机

接着安装 `Vagrant`

下载地址 [Vagrant下载](https://www.vagrantup.com/downloads.html)

下载好对应系统的软件, 双击安装, 一路下一步即可, 安装完成之后, 重启电脑

安装完成之后, 检查一下是否安装完成, `cmd` 中运行:

```
vagrant --version
```

![](http://cdn.wkbook.cn/img/20201117003132.png)

**安装 Homestead Vagrant Box**

> 注意: `安装 Homestead Vagrant Box` 这里可以跳过,跳过,跳过, 这里安装版本可能和后面 `vagrant up` 要使用的版本不一致, 导致要重新安装

`cmd` 执行命令

```
vagrant box add laravel/homestead
```

之后会让你选择所使用的虚拟机, 输入3, 选择`virtualbox`, 回车:

![](http://cdn.wkbook.cn/img/20201117003610.png)

之后等待下载完成即可, 根据你的网速, 可能需要一定的时间

查看是否安装完成:

`cmd` 输入 `vagrant box list`

![](http://cdn.wkbook.cn/img/20201117012651.png)

> 注意: 再次强调, `安装 Homestead Vagrant Box` 这里可以跳过,跳过,跳过, 这里安装版本可能和后面 `vagrant up` 要使用的版本不一致, 导致要重新安装

**安装 Homestead**

下载地址:

[Homestead下载](https://github.com/laravel/homestead/archive/master.zip)

> 因为没有学Git, 所以这里就不使用 Git 克隆了, 而是直接下载压缩包

解压到家目录下, 改目录名为 `Homestead`:

![](http://cdn.wkbook.cn/img/20201117012021.png)

 `cmd` 中进入到这个目录, 执行 `init.bat` 命令以创建 Homestead.yaml 配置文件。Homestead.yaml 将会置于 Homestead 目录中:

```
init.bat
```

![](http://cdn.wkbook.cn/img/20201117012906.png)

**修改配置文件**

修改 `~/Homestead/Homestead.yaml` 配置文件:

配置共享文件夹, 在桌面创建 `workspack` 目录, 用于我们的工程目录, 并在 `workspack` 目录里面创建 `demo` 项目:

```
folders:
    - map: C:\Users\m1339\Desktop\wokspace\demo
      to: /home/vagrant/code/demo
```

> 共享文件夹是为了将你本机的某个文件夹里面的代码, 同步到虚拟机中的某个目录

> 注意：Windows 不要使用 ~/ 路径语法，而应该使用项目的完整路径，如 C:\Users\m1339\Desktop\wokspace\demo

> 您应该始终将各个项目映射到它们自己的文件夹映射，而不是映射整个 C:\Users\m1339\Desktop\wokspace 文件夹。映射文件夹时，虚拟机保持跟踪文件夹中 每一个 文件的所有磁盘 IO。当文件夹中有大量文件时，此举可能影响性能。

配置 Nginx 站点:

sites 属性将允许您轻松的将「域名」映射到 Homstead 环境中的目录中去。Homestead.yaml 中包含了一个简单的站点配置。同样，您可以按需添加许多站点到您的 Homestead 环境中。

```
sites:
    - map: demo.test
      to: /home/vagrant/code/demo
```

如果您在启动了 Homestead box 后修改了 site 属性，您需要再次运行 vagrant reload --provision 命令以更新虚拟机中的 Nginx 配置

> 注意：Homestead 脚本设计时，尽可能保持操作的幂等。当然，如果你在 provisioning 过程中遇到了问题，您可以通过 vagrant destroy && vagrant up 销毁和重构虚拟机。

域名解析:

修改 `C:\Windows\System32\drivers\etc\hosts` 。添加到 hosts 文件中的记录如下：

```
192.168.10.10 demo.test
```

> ip 为 Homestead.yaml 中配置的ip

**启动 Vagrant Box**

启动命令为, 在 `Homestead` 目录下执行 cmd 命令:

```
vagrant up
```

> 在启动动的过程中, 会进行公钥和私钥的认证, 请确保你已在家目录中的 `.ssh` 生成了私钥和公钥

最后在工程目录 `workspace/demo` 目录下创建 `index.html` 文件, 并写入代码:

```
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Welcome to demo.test</title>
  </head>
  <body>
    <h1>Success! demo.test home page!</h1>
  </body>
</html>
```

在浏览器中访问 `demo.test`:

**ssh连接到虚拟机**

执行 `cmd` 命令:

```
vagrant ssh
```

**关闭虚拟机**

如果暂时不用这个虚拟机了, 可以关闭虚拟机:

```
vagrant halt
```

**销毁虚拟机**

如果暂时不需要这个虚拟机了, 可以销毁虚拟机, 执行 `cmd` 命令:

```
vagrant destroy --force
```

> 改名了会销毁虚拟机, 慎用

## Vagrant 常用命令

- vagrant box list  查看目前已有的box
- vagrant box add 新增加一个box
- vagrant box remove  删除指定box
- vagrant init  初始化配置vagrantfile
- vagrant up  启动虚拟机
- vagrant ssh ssh登录虚拟机
- vagrant suspend 挂起虚拟机
- vagrant reload  重启虚拟机
- vagrant halt  关闭虚拟机
- vagrant status  查看虚拟机状态
- vagrant destroy 删除虚拟机
- vagrant up --provision 重载配置