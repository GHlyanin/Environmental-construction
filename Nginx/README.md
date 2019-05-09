# Nginx 编译安装

![nginx_0002](https://github.com/GHlyanin/Environmental-construction/blob/master/Nginx/image/nginx_0002.PNG)![nginx_0003](https://github.com/GHlyanin/Environmental-construction/blob/master/Nginx/image/nginx_0003.PNG)![nginx_0004](https://github.com/GHlyanin/Environmental-construction/blob/master/Nginx/image/nginx_0004.PNG)

[Nginx](http://nginx.org/en/) 是一款轻量级的、开源的、高性能的 HTTP 服务器和反向代理服务器，本文主要介绍 Nginx 在 [Ubuntu 18.04.2](https://www.ubuntu.com/download/desktop) 系统中的编译安装过程，经测试该过程适应目前 Nginx 官网所有下载版本（nginx-0.5.38 至 nginx-1.16.0），只是对于 nginx-1.12.2 以下版本，在编译时需要少许设置

Nginx 源代码可以从其官网获取限定版本，或者搜索 `index of nginx` 和 `index of php` 从开源网站获取指定版本

- [Nginx Download](http://nginx.org/en/download.html)

------

## 安装编译平台

[Ubuntu](https://www.ubuntu.com) 缺省情况下，没有提供 C/C++ 的编译环境，但是 Ubuntu 提供了一个 [build-essential](https://code.launchpad.net/ubuntu/+source/build-essential) 软件包，安装了该软件包，编译 C/C++ 所需要的软件包都会被安装  

libtool 是一个通用库支持脚本，将使用动态库的复杂性隐藏在统一、可移植的接口中，主要的一个作用是在编译大型软件的过程中解决了库的依赖问题

```
sudo apt-get install build-essential
sudo apt-get install libtool
```

------

## 安装 Nginx

Nginx 源代码安装有两种方式：离线下载安装和在线下载安装，此处以在线下载安装为例，演示 Nginx 的编译安装过程

### 0x01 安装依赖库

Nginx 是高度自由化的 web 服务器，它的功能是由许多模块来支持，此处安装的 Nginx，是最简化版的 Nginx，仅仅支持 Nginx 的基本功能，如果需要更多的功能，可以重新编译安装更多的模块  

不同模块的编译安装，需要安装不同的依赖库，此处安装的 Nginx 需要的依赖库是三个

- pcre 库：支持重写 rewrite 功能
- zlib 库：支持 gzip 压缩
- openssl 库：支持 ssl 功能

此处安装的依赖库是系统自带的版本，如果需要安装其他版本，可以单独下载编译安装，但需要注意的是在 Nginx 编译安装时，需要把单独安装的依赖库加载进去（使用 `--with` 加载）。下面是系统自带依赖库安装过程

```
sudo apt-get install libpcre3 libpcre3-dev  
sudo apt-get install zlib1g-dev
sudo apt-get install openssl libssl-dev 
```

### 0x02 编译安装 Nginx

**Step 1**：在线下载 `nginx-xxx` 压缩包、解压该压缩包和进入该解压目录

```
wget http://nginx.org/download/nginx-xxx.tar.gz
tar -zxvf nginx-xxx.tar.gz
cd nginx-xxx
```

**Step 2**：进行编译安装（老版本编译安装会报 `-Werror` 错误，解决方法参考[附录 1](#附录-1)）

```
sudo ./configure
sudo make
sudo make install
```

**默认安装路径**

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

**主要文件地址**

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


### 0x03 Nginx 配置和启动

目前安装的 Nginx，已经能正常解析 HTML 页面，但是仍然不能解析 PHP 页面，需要后续安装 PHP 解释器，为了支持后续的 PHP 解析，此时先对 Nginx 进行配置，不同需求的 Nginx，最终的配置文件不同，此处仅仅给出简单的 Nginx 配置

**Step 1**：配置`nginx.conf`文件

```
sudo vim /usr/local/nginx/conf/nginx.conf
```

> 去除 `location ~ \.php$` 前的分号，并修改如下

```php
location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
```

**Step 2**：配置修改之后，对配置文件的语法进行检查，并开启 Nginx服务（Nginx 常用命令参考[附录 2](#附录-2)）

```
sudo /usr/local/nginx/sbin/nginx -t
sudo /usr/local/nginx/sbin/nginx
```

**Step 3**：访问本机地址，看到如下页面，表示 Nginx 安装成功

![nginx_0001](https://github.com/GHlyanin/Environmental-construction/blob/master/Nginx/image/nginx_0001.PNG)

------

## 附录

### 附录 1

经过测试，当版本低于 nginx-1.12.2 时，在 Ubuntu 18.04.2 系统中可以正常 `sudo ./configure`，但是当使用 `sudo make` 时，会出现类似于如下的错误（nginx-1.2.9 编译错误示例）

```makefile
src/core/ngx_murmurhash.c: In function ‘ngx_murmur_hash2’:
src/core/ngx_murmurhash.c:37:11: error: this statement may fall through [-Werror=implicit-fallthrough=]
         h ^= data[2] << 16;
         ~~^~~~~~~~~~~~~~~~
src/core/ngx_murmurhash.c:38:5: note: here
     case 2:
     ^~~~
src/core/ngx_murmurhash.c:39:11: error: this statement may fall through [-Werror=implicit-fallthrough=]
         h ^= data[1] << 8;
         ~~^~~~~~~~~~~~~~~
src/core/ngx_murmurhash.c:40:5: note: here
     case 1:
     ^~~~
cc1: all warnings being treated as errors
objs/Makefile:429: recipe for target 'objs/src/core/ngx_murmurhash.o' failed
make[1]: *** [objs/src/core/ngx_murmurhash.o] Error 1
make[1]: Leaving directory '/home/ubuntu/Desktop/nginx-1.2.9'
Makefile:8: recipe for target 'build' failed
make: *** [build] Error 2
```

**解决方法**：  

**Step 1**：在解压目录下正常配置

```
sudo ./configure
```

**Step 2**：修改 `objs/Makefile` 文件，去除 `-Werror` 参数

```
sudo vim objs/Makefile 
```

> 原版 `objs/Makefile` 

```
CC =    gcc
CFLAGS =  -O -pipe  -O -W -Wall -Wpointer-arith -Wno-unused -Werror -g
CPP =   gcc -E
LINK =  $(CC)
```

> 修改 `objs/Makefile` 为

```
CC =    gcc
CFLAGS =  -O -pipe  -O -W -Wall -Wpointer-arith -Wno-unused -g
CPP =   gcc -E
LINK =  $(CC)
```

**Step 3**：正常编译安装即可

```
sudo make
sudo make install
```

### 附录 2

Nginx 程序安装目录在 `/usr/local/nginx/sbin/`，以下给出指定安装目录的 Nginx 常用命令

- **检查 `nginx` 语法**

```
sudo /usr/local/nginx/sbin/nginx -t
```

> 如果返回如下内容，则表示 `nginx` 语法无误

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

- **版本**

```
sudo /usr/local/nginx/sbin/nginx -v/-V
```


