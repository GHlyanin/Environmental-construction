# Nginx + PHP 编译安装

Nginx本身不能处理PHP脚本，它只是一个web服务器，如果收到的请求是PHP，则需要调用PHP解释器进行处理，本文主要介绍Nginx和PHP的在Ubuntu系统中的编译安装过程，Nginx和PHP源代码可以从其官网获取限定版本，或者搜索`index of nginx`和`index of php`从开源网站获取指定版本

- [Nginx官网](http://nginx.org/en/download.html)
- [PHP官网](https://www.php.net/downloads.php)

## 安装编译平台

Ubuntu缺省情况下，没有提供C/C++的编译环境，但是Ubuntu提供了一个`build-essential`软件包，安装了该软件包，编译c/c++所需要的软件包都会被安装；`libtool`是一个通用库支持脚本，将使用动态库的复杂性隐藏在统一、可移植的接口中，主要的一个作用是在编译大型软件的过程中解决了库的依赖问题

```
sudo apt-get install build-essential
sudo apt-get install libtool
```

## 安装Nginx

> Nginx源代码安装有两种方式：离线下载安装和在线下载安装，此处以在线下载安装为例，演示Nginx的编译安装过程

### 0x01 安装依赖库

> Nginx是高度自由化的web服务器，它的功能是由许多模块来支持，此处安装的Nginx，是最简化版的Nginx，仅仅支持Nginx的基本功能，如果需要更多的功能，可以重新编译安装更多的模块。不同模块的编译安装，需要安装不同的依赖库，此处安装的Nginx需要的依赖库是三个：

- `pcre`库：支持重写rewrite功能
- `zlib`库：支持gzip压缩
- `openssl`库：支持ssl功能

> 此处安装的依赖库是系统自带的版本，如果需要安装最新版本，可以单独下载编译安装，但需要注意的是在Nginx编译安装时，需要把单独安装的依赖库加载进去（使用`--with`加载）。下面是系统自带依赖库安装过程

```
sudo apt-get install libpcre3 libpcre3-dev  
sudo apt-get install zlib1g-dev
sudo apt-get install openssl libssl-dev 
```

### 0x02 编译安装Nginx

> 在线下载`nginx-xxx`压缩包、解压该压缩包和进入该解压目录

```
wget http://nginx.org/download/nginx-xxx.tar.gz
tar -zxvf nginx-xxx.tar.gz
cd nginx-xxx
```

> 进行编译安装

```
sudo ./configure
sudo make
sudo make install
```

> 默认安装路径为

```
nginx path prefix: "/usr/local/nginx"
nginx binary file: "/usr/local/nginx/sbin/nginx"
nginx modules path: "/usr/local/nginx/modules"
nginx configuration prefix: "/usr/local/nginx/conf"
nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
nginx pid file: "/usr/local/nginx/logs/nginx.pid"
nginx error log file: "/usr/local/nginx/logs/error.log"
nginx http access log file: "/usr/local/nginx/logs/access.log"
```

> 主要文件地址

- 配置文件：

```
/usr/local/nginx/conf/nginx.conf
```

- 程序文件：

```
/usr/local/nginx/sbin/nginx
```

- 网站目录：

```
/usr/local/nginx/html/
```


### 0x03 Nginx配置和启动

> 目前安装的Nginx，已经能正常解析HTML页面，但是仍然不能解析PHP页面，需要后续安装PHP解释器，为了支持后续的PHP解析，此时先对Nginx进行配置，不同需求的Nginx，最终的配置文件不同，此处仅仅给出简单的Nginx配置

```
sudo vim /usr/local/nginx/conf/nginx.conf
```

> 修改`nginx.conf`如下

```
location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
```

> 配置修改之后，对配置文件的语法进行检查，并开启Nginx服务（[Nginx常用命令](#0x04-Nginx常用命令)）

```
sudo /usr/local/nginx/sbin/nginx -t
sudo /usr/local/nginx/sbin/nginx
```

> 访问本机地址，看到如下页面，表示Nginx安装成功

![nginx_php_0001](https://github.com/GHlyanin/Environmental-construction/blob/master/Ubuntu_nginx_php/image/nginx_php_0001.PNG)

### 0x04 Nginx常用命令

> Nginx程序安装目录在`/usr/local/nginx/sbin/`，以下给出指定安装目录命令

- **检查`nginx`语法**

```
sudo /usr/local/nginx/sbin/nginx -t
```

> 如果返回如下内容，则表示`nginx`语法无误

```
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```

- **启动**

```
sudo /usr/local/nginx/sbin/nginx
```

- **关闭**

```
sudo /usr/local/nginx/sbin/nginx -s stop
```

- **重启**

```
sudo /usr/local/nginx/sbin/nginx -s reload
```

- **查看版本信息**

```
sudo /usr/local/nginx/sbin/nginx -v/-V
```

## 安装PHP

> Nginx解析PHP脚本，需要安装PHP和PHP-FPM进程管理器，PHP5.3版本以后，PHP-FPM已经正式内置在PHP中，不再是第三方补丁包。PHP源代码安装有两种方式：离线下载安装和在线下载安装，此处以离线下载安装为例，演示PHP的编译安装过程

### 0x01 安装依赖库

> 此处安装的PHP，是最简化版的PHP，仅仅支持PHP的基本功能，如果需要更多的功能，可以重新编译安装更多的模块。不同模块的编译安装，需要安装不同的依赖库，此处安装的PHP需要一个依赖库：

```
sudo apt-get install libxml2 libxml2-dev
```

### 0x02 编译安装PHP

> 离线下载`php-xxx`压缩包、解压该压缩包和进入该解压目录

```
tar -zxvf php-xxx
cd php-xxx/
```

> 进行编译安装

```
sudo ./configure --enable-fpm
sudo make
sudo make install
```

> 初始化配置文件

```
sudo cp php.ini-production /usr/local/lib/php.ini
sudo cp /usr/local/etc/php-fpm.conf.default /usr/local/etc/php-fpm.conf
sudo cp /usr/local/etc/php-fpm.d/www.conf.default /usr/local/etc/php-fpm.d/www.conf

```

:pencil:**Thinking**

> 在编译安装PHP时，如果没有生成`php.ini`文件，可以从PHP编译安装包中复制`php.ini-production`并重命名为`php.ini`，然后把该`php.ini`放到默认安装位置，重新启动即可载入`php.ini`文件。此处有三点说明：
1. PHP编译安装包中有`php.ini-development`和`php.ini-production`配置文件，前者适合开发环境，后者适合生产环境，后者稳定性更强，因此选择后者
2. `php.ini`默认安装位置的查找，在网站目录下，建立一个内容如下的`phpinfo.php`文件，打开浏览器访问该文件，`Configuration File (php.ini) Path`指示的路径就是`php.ini`默认安装位置，把`php.ini`放在该位置下重新启动即可
3. `php.ini`文件的复制和配置，推荐PHP文件能正常解析之后再操作

```
<?php
 echo phpinfo();
?>
```

> 默认安装路径为

```
Installing PHP CLI binary:        /usr/local/bin/
Installing PHP CLI man page:      /usr/local/php/man/man1/
Installing PHP FPM binary:        /usr/local/sbin/
Installing PHP FPM config:        /usr/local/etc/
Installing PHP FPM man page:      /usr/local/php/man/man8/
Installing PHP FPM status page:      /usr/local/php/php/fpm/
Installing phpdbg binary:         /usr/local/bin/
Installing phpdbg man page:       /usr/local/php/man/man1/
Installing PHP CGI binary:        /usr/local/bin/
Installing PHP CGI man page:      /usr/local/php/man/man1/
Installing build environment:     /usr/local/lib/php/build/
Installing header files:          /usr/local/include/php/
Installing helper programs:       /usr/local/bin/
```

> 主要文件地址

- PHP配置文件：

```
/usr/local/lib/php.ini
```

- PHP-FPM配置文件：

```
/usr/local/etc/php-fpm.conf
/usr/local/etc/php-fpm.d/www.conf
```

- 程序文件：

```
/usr/local/sbin/php-fpm
```

### 0x03 PHP-FPM配置和启动

> **Step 1**：配置`php-fpm.conf`

```
sudo vim /usr/local/etc/php-fpm.conf
```

> 去除`pid`前的分号，让其生成`php-fpm.pid`，从而支持PHP-FPM信号控制

```
pid = run/php-fpm.pid
```

> 修改最后一行为

```
include=/usr/local/etc/php-fpm.d/*.conf
```

> **Step 2**：配置`www.conf`文件

```
sudo vim /usr/local/etc/php-fpm.d/www.conf
```

> 修改PHP-FPM运行用户

```
user = www-data
group = www-data
```

> 如果用户不存在，那么先添加`www-data`用户

```
groupadd www-data  
useradd -g www-data www-data
```

> **Step 3**：启动PHP-FPM服务（[PHP-FPM常用命令](#0x04-php-fpm常用命令)）

```
sudo /usr/local/sbin/php-fpm
```

> **step 4**：建立一个`phpinfo.php`文件，访问该文件，看到如下页面，表示整体环境搭建成功

![nginx_php_0002](https://github.com/GHlyanin/Environmental-construction/blob/master/Ubuntu_nginx_php/image/nginx_php_0002.PNG)

### 0x04 PHP-FPM常用命令

> PHP5.3.3以后的PHP-FPM不再支持`/usr/local/sbin/php-fpm (start|stop|reload)`等命令，需要使用信号控制

- **启动**

```
sudo /usr/local/sbin/php-fpm
```

- **查看PHP-FPM进程（master process）**

```
ubuntu@ubuntu-virtual-machine:~$ ps aux |grep php-fpm
root      14506  0.0  0.3  94736  7012 ?        Ss   11:03   0:00 php-fpm: master process (/usr/local/etc/php-fpm.conf)
www-data  14507  0.0  0.3  97036  6976 ?        S    11:03   0:00 php-fpm: pool www
www-data  14508  0.0  0.3  97036  6976 ?        S    11:03   0:00 php-fpm: pool www
ubuntu    14510  0.0  0.0  21536  1064 pts/0    S+   11:03   0:00 grep --color=auto php-fpm
```

- **关闭**

> 使用进程关闭

```
sudo kill -INT 14506
```

> 使用`php-fpm.pid`关闭，注意命令中不是单引号，是反单引号，即重音符

```
sudo kill -INT `cat /usr/local/var/run/php-fpm.pid`
```

- **重启**

> 使用进程重启

```
sudo kill -USR2 14506
```

> 使用`php-fpm.pid`重启，注意命令中不是单引号，是反单引号，即重音符

```
sudo kill -USR2 `cat /usr/local/var/run/php-fpm.pid`
```











