ubuntu14.04安装 nginx-1.8.0
==================
需要安装如下包：

    apt-get install build-essential
    apt-get install libtool
需要先装pcre, zlib，pcre为了重写rewrite，zlib为了gzip压缩;
可以是任何目录，本文选定的是/usr/local/src

    cd /usr/local/src
1.安装PCRE库

    cd /usr/local/src
    wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz 
    tar -zxvf pcre-8.37.tar.gz
    cd pcre-8.37
    ./configure
    make
    make install
2.安装zlib库

    cd /usr/local/src
    wget http://zlib.net/zlib-1.2.8.tar.gz
    tar -zxvf zlib-1.2.8.tar.gz
    cd zlib-1.2.8
    ./configure
    make
    make install
3.安装ssl（如果没有安装的话）

    cd /usr/local/src
    wget http://www.openssl.org/source/openssl-1.0.1c.tar.gz
    tar -zxvf openssl-1.0.1c.tar.gz
如果在linux下安装openssl，执行config和make之后，在执行make install时如果出现下面的错误

    cms.pod around line 457: Expected text after =item, not a number 
    cms.pod around line 461: Expected text after =item, not a number 
    cms.pod around line 465: Expected text after =item, not a number 
    cms.pod around line 470: Expected text after =item, not a number 
    cms.pod around line 474: Expected text after =item, not a number 
    POD document had syntax errors at /usr/bin/pod2man line 69. 
则在root权限下，执行

    rm -f /usr/bin/pod2man  
然后重新安装

    make install
4.安装nginx

    cd /usr/local/src
    wget http://nginx.org/download/nginx-1.8.0.tar.gz
    tar -zxvf nginx-1.8.0.tar.gz
    cd nginx-1.8.0
    
    ./configure --sbin-path=/usr/local/nginx/nginx \
    --conf-path=/usr/local/nginx/nginx.conf \
    --pid-path=/usr/local/nginx/nginx.pid \
    --with-http_ssl_module \
    --with-pcre=/usr/local/src/pcre-8.37 \
    --with-zlib=/usr/local/src/zlib-1.2.8 \
    --with-openssl=/usr/local/src/openssl-1.0.1c
Configuration summary 配置结果如下：

     + using PCRE library: /usr/local/src/pcre-8.37
     + using OpenSSL library: /usr/local/src/openssl-1.0.1c
     + md5: using OpenSSL library
     + sha1: using OpenSSL library
     + using zlib library: /usr/local/src/zlib-1.2.8

     nginx path prefix: "/usr/local/nginx"
     nginx binary file: "/usr/local/nginx/nginx"
     nginx configuration prefix: "/usr/local/nginx"
     nginx configuration file: "/usr/local/nginx/nginx.conf"
     nginx pid file: "/usr/local/nginx/nginx.pid"
     nginx error log file: "/usr/local/nginx/logs/error.log"
     nginx http access log file: "/usr/local/nginx/logs/access.log"
     nginx http client request body temporary files: "client_body_temp"
     nginx http proxy temporary files: "proxy_temp"
     nginx http fastcgi temporary files: "fastcgi_temp"
     nginx http uwsgi temporary files: "uwsgi_temp"
     nginx http scgi temporary files: "scgi_temp"
最后编译安装

     make
     make install
5.启动配置

    [root@localhost~]# vi /etc/init.d/nginx
    #!/bin/bash
    # chkconfig: 3459920
    # description: Nginx servicecontrol script
    PROG="/usr/local/nginx/nginx"
    PIDF="/usr/local/nginx/logs/nginx.pid"
    case"$1"in
    start)
    $PROG
    echo "Nginx servicestart success."
    ;;
    stop)
    kill -s QUIT $(cat $PIDF)
    echo "Nginx service stopsuccess."
    ;;
    restart)
    $0stop
    $0start
    ;;
    reload)
    kill -s HUP $(cat $PIDF)
    echo"reload Nginx configsuccess."
    ;;
    *)
    echo "Usage: $0{start|stop|restart|reload}"
    exit 1
    esac
