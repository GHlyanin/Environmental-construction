# Ubuntu 编译安装 Nginx + PHP

> Nginx本身不能处理PHP脚本，它只是一个web服务器，如果收到的请求是PHP，则需要调用PHP解释器进行处理，本文主要介绍Nginx和PHP的编译安装过程，Nginx和PHP源代码可以从其官网获取限定版本，或者搜索`index of nginx`和`index of php`从开源网站获取指定版本

- [Nginx官网](http://nginx.org/en/download.html)
- [PHP官网](https://www.php.net/downloads.php)

## 安装编译平台

> Ubuntu缺省情况下，没有提供C/C++的编译环境，但是Ubuntu提供了一个`build-essential`软件包，安装了该软件包，编译c/c++所需要的软件包都会被安装；`libtool`是一个通用库支持脚本，将使用动态库的复杂性隐藏在统一、可移植的接口中，主要的一个作用是在编译大型软件的过程中解决了库的依赖问题

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

> 下载`nginx-xxx`压缩包、解压该压缩包和进入该解压目录

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

```
配置文件：/usr/local/nginx/conf/nginx.conf
程序文件：/usr/local/nginx/sbin/nginx
网站目录：/usr/local/nginx/html/
```

### 0x03 Nginx常用命令

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

:pencil:**Thinking**  

> 根据以上三个步骤，就能正常开启Nginx服务。目前的Nginx服务器只能解析HTML静态页面，如果要解析PHP动态脚本，需要安装PHP解释器

## PHP安装与配置





```
sudo apt-get install libxml2
sudo apt-get install libxml2-dev


sudo make
sudo make install





sudo apt-get install libxml2-dev
sudo apt-get install build-essential
sudo apt-get install openssl 
sudo apt-get install libssl-dev 
sudo apt-get install make
sudo apt-get install curl
sudo apt-get install libcurl4-gnutls-dev
sudo apt-get install libjpeg-dev
sudo apt-get install libpng-dev
sudo apt-get install libmcrypt-dev
sudo apt-get install libreadline6 libreadline6-dev
```





```
include=/usr/local/etc/php-fpm.d/*.conf
```



```
location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
```

```
sudo cp /usr/local/etc/php-fpm.conf.default /usr/local/etc/php-fpm.conf
sudo cp /usr/local/etc/php-fpm.d/www.conf.default /usr/local/etc/php-fpm.d/www.conf




```

