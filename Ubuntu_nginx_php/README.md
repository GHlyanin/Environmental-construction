# Ubuntu Nginx PHP





## Nginx安装与配置

> 本文使用源代码对Nginx进行安装，源代码可以从[Nginx官网](http://nginx.org/en/download.html)下载安装包离线安装，也可以使用`wget`命令指定版本在线安装。本文以`nginx-1.14.2`为例演示Nginx安装过程，该安装过程总共分为三个步骤：

- [配置Nginx安装环境](0x01-Nginx安装环境)
- [编译安装Nginx源代码](#0x02-Nginx编译安装)
- [启动Nginx服务](#0x03-Nginx常用命令)


### 0x01 Nginx安装环境

> 安装`gcc`编译器

```
sudo apt-get install gcc
```

> 安装`pcre`库、`zlib`库、`openssl`库

```
sudo apt-get install libpcre3 libpcre3-dev  
sudo apt-get install zlib1g-dev
sudo apt-get install openssl libssl-dev 
```

### 0x02 Nginx编译安装

> 下载`nginx-1.14.2`压缩包、解压该压缩包和进入该解压目录

```
wget http://nginx.org/download/nginx-1.14.2.tar.gz
tar -zxvf nginx-1.14.2.tar.gz
cd nginx-1.14.2
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

