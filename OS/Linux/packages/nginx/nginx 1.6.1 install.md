# Nginx 编译安装
### Version:nginx-1.16.1
### 说明：在国产银河麒麟操作系统上默认使用的是这个版本的Nginx，以此版本为例说明安装过程。

#### 安装需求：支持cookie、支持ssl、http request header转发

步骤：
1. 下载相关源码
   1. Nginx源码：[下载连接](https://nginx.org/download/nginx-1.16.1.tar.gz)
   2. Sticky：[下载链接](https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/get/master.tar.gz)
2. 依赖
   1. zlib-devel
   2. openssl-devel
   3. pcre-devel
3. 解压
   1. tar -xvf nginx-1.16.1.tar.gz
   2. tar -xvf master.tar.gz <font color=red>如果解压后的文件名称过长，则重名称文件夹。</font>
4. configure
   1. nginx-sticky-module 移动到/usr/local/nginx-sticky-module
   2. 将目录切换到nginx源码目录。
   3. 执行./configure --prefix=/opt/nginx
    --sbin-path=/opt/nginx
    --conf-path=/opt/nginx.conf
    --pid-path=/opt/nginx.pid
    --with-http_ssl_module
    --add-module=/usr/local/nginx-sticky-module
    * <font color=red>特别提醒：一开始安装的nginx-sticky-module的时候一直在nginx-sticky-module目录下面找configure文件。导致一致找不到，实际上nginx-sticky-module是要nginx源码一起编译安装的。</font>
5. 编译安装：在nginx源码目录下执行make && make install

### 安装命令汇总
``` shell{.line-numbers}
~# tar -xvf nginx-1.16.1.tar.gz
~# tar -xvf master.tar.gz
~# mv nginx-goodies-nginx-sticky-module-ng-08a395c66e42 /usr/local/nginx-sticky-module
~# dnf install zlib-devel openssl-devel pcre-devel
~# cd nginx-1.16.1
~# ./configure --prefix=/opt/nginx --add-module=/usr/local/nginx-sticky-module --with-http_ssl_module
~# make && make install
~# cd /opt/nginx
~# vim conf/nginx.conf
```
 
 # 安装问题解决办法：
 ###  sticky问题，ngx_http_sticky_misc.c:176:15: 错误：‘SHA_DIGEST_LENGTH’未声明(在此函数内第一次使用)
 * 处理方式：
   <font color=green>安装openssl-devel即可。如果openssl-devel安装后问题还是存在，则修改如下sticky源码文件：</font>
```shell{.line-numbers}
~# vim /usr/local/nginx-sticky-module/ngx_http_sticky_misc.c
```
*  <font color=red> 添加文件
  #include <openssl/sha.h>
  #include <openssl/md5.h></font>

```c{.line-numbers}
#include <nginx.h>
#include <ngx_config.h>
#include <ngx_core.h>
#include <ngx_http.h>
#include <ngx_md5.h>
#include <ngx_sha1.h>
#include <openssl/sha.h> //添加头文件1
#include <openssl/md5.h> //添加头文件2

#include "ngx_http_sticky_misc.h"
```
### Nginx启动服务器时出现 [emerg] https protocol requires SSL support in XXXXXX
 * 处理方式：
   <font color=green>重新配置，编译安装。添加--with-http_ssl_module</font>
``` shell{.line-numbers}
~# ./configure --prefix=/opt/nginx --add-module=/usr/local/nginx-sticky-module --with-http_ssl_module
```

### 解决nginx反向代理proxy不能转发header报头

使用nginx做负载均衡或http代理时，碰到http header不转发的问题。
配置里只有转发设置原始ip和host的

proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header     X-Forwarded-Server $host;

### 自定义header不能转发问题

恰好我自定义的header中都是用的下划线。

处理办法： 

1. 配置中http部分 增加underscores_in_headers on; 配置
2. 用减号-替代下划线符号_，避免这种变态问题。nginx默认忽略掉下划线可能有些原因。
可以加到http或者server中
语法：underscores_in_headers on|off
默认值：off
使用字段：http, server
是否允许在header的字段中带下划线。

### 完整配置文件示例
``` nginx {.line-numbers}

user  root;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    # 解决不能转发带下划线的http头变量问题
	underscores_in_headers on;
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    upstream cl_test { 
        # 转发cookie
		sticky expires=1d secure path=/;
		server 10.8.8.14:8228;
	}

    server {
        listen       80;
        server_name  _;
        # 转发http请求头
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-for $proxy_add_x_forwarded_for;
		#charset koi8-r;

        #access_log  logs/host.access.log  main;
        #
        location /sign {
			proxy_pass http://10.8.8.14:9999/sign/;
		}
        location / {
            # 转发http请求头 这里比较关键
			proxy_set_header Host cl_test;
        	proxy_pass https://cl_test;
		    #    root   html;
		   index  default.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```