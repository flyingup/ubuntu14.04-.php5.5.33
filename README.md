ubuntu14.04 安装php5.5.33
==========================
下载php-5.5.15.tar.gz源码文件

[php](http://php.net/downloads.php#v5.5.15)

解压php-5.5.15.tar.gz

    tar -xvf php-5.5.15.tar.gz
进入解压之后的php源码目录

    cd php-5.5.15
配置.config

    ./configure
    --prefix=/usr/local/php/ 
    --with-mysql 
    --enable-fpm 
    --enable-debug 
    --with-config-file-path=/usr/local/php/etc 
    --with-libxml-dir 
    --with-openssl 
    --with-pcre-regex 
    --with-zlib 
    --with-curl 
    --with-gd 
    --enable-zip 
    --enable-mbstring 
    --with-mhash 
    --with-jpeg-dir 
    --enable-gd-native-ttf 
    --with-png-dir 
    --with-freetype-dir 
    --enable-mbstring 
    --enable-fpm 
    --with-fpm-user=www 
    --with-fpm-group=www 
    --with-mcrypt
常见错误：由于库的安装不完整导致的，根据提示就能知道安装哪些包。
==========================================================
1.Configure: error: xml2-config not found. Please check your libxml2 installation.

    sudo apt-get install libxml2-dev
    
2.configure: error: Cannot find libz

     sudo apt-get install libzip-dev

3.ubuntu14.04无法安装Curl

      sudo add-apt-repository ppa:costamagnagianfranco/ettercap-stable-backports
      sudo apt-get update
      sudo apt-get install curl

4.configureerror:please reinstall the libcurl distribution - easy.h should be in<curl-dir>/include/curl/

     sudo apt-get install libcurl4-gnutls-dev

5.configure: error: jpeglib.h not found.

    sudo apt-get install libjpeg-dev
6.configure: error: png.h not found.

     sudo apt-get install libpng-dev
7.configure: error: freetype.h not found.

    sudo apt-get install libfreetype6-dev

8.configure: error:Cannot find MySQL header files under /usr/lib/mysql
Note that the MySQL Client library isnot boundled anymore.

    sudo apt-get install mysql-client
9.configure: error: mcrypt.h not found. Please reinstall libmcrypt.

    sudo apt-get install libmcrypt-dev
  
10.Cannot find OpenSSL's <evp.h>

      sudo apt-get install libssl-dev
成功执行的话进行编译

    make
编译完成进行安装

    sudo make install
