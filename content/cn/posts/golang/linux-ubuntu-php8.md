

### 获取并解压 PHP 源代码:

```sh
php-8.1.0alpha2.tar.bz2	2021-06-22 16:59	14M	 
php-8.1.0alpha2.tar.gz	2021-06-22 16:59	18M	 
php-8.1.0alpha2.tar.xz	2021-06-22 16:59	11M	 
```

下载 php-8.1.0alpha2.tar.xz 

wget https://downloads.php.net/~patrickallaert/php-8.1.0alpha2.tar.xz


解压 xz 格式文件


方法一：
需要用到两步命令，首先利用 xz-utils 的 xz 命令将 linux-3.12.tar.xz 解压为 linux-3.12.tar，其次用 tar 命令将 linux-3.12.tar 完全解压。
 
xz -d php-8.1.0alpha2.tar.xz
tar -xf linux-3.12.tar
 
 
方法二（推荐）

tar -Jxf linux-3.12.tar.xz



报错: No package 'libxml-2.0' found
解决办法: 

```sh
$ sudo apt install libxml2
$ sudo apt install libxml2-dev
```

No package 'sqlite3' found



### 配置并构建 PHP。

在此步骤您可以使用很多选项自定义 PHP，例如启用某些扩展等。 运行 ./configure --help 命令来获得完整的可用选项清单。 在本示例中，我们仅进行包含 PHP-FPM 和 MySQL 支持的简单配置。



./configure --enable-fpm --with-mysql

make
make install

 
```sh
$ make install
Installing shared extensions:     /usr/local/lib/php/extensions/no-debug-non-zts-20201009/
Installing PHP CLI binary:        /usr/local/bin/
Installing PHP CLI man page:      /usr/local/php/man/man1/
Installing PHP FPM binary:        /usr/local/sbin/
Installing PHP FPM defconfig:     /usr/local/etc/
Installing PHP FPM man page:      /usr/local/php/man/man8/
Installing PHP FPM status page:   /usr/local/php/php/fpm/
Installing phpdbg binary:         /usr/local/bin/
Installing phpdbg man page:       /usr/local/php/man/man1/
Installing PHP CGI binary:        /usr/local/bin/
Installing PHP CGI man page:      /usr/local/php/man/man1/
Installing build environment:     /usr/local/lib/php/build/
Installing header files:          /usr/local/include/php/
Installing helper programs:       /usr/local/bin/
  program: phpize
  program: php-config
Installing man pages:             /usr/local/php/man/man1/
  page: phpize.1
  page: php-config.1
/root/softwares/php-8.1.0alpha2/build/shtool install -c ext/phar/phar.phar /usr/local/bin/phar.phar
ln -s -f phar.phar /usr/local/bin/phar
Installing PDO headers:           /usr/local/include/php/ext/pdo/
```



/usr/local/bin/php -v
PHP 8.1.0alpha2 (cli) (built: Jun 23 2021 17:52:07) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.0-dev, Copyright (c) Zend Technologies


 php-fpm -v
PHP 8.1.0alpha2 (fpm-fcgi) (built: Jun 23 2021 17:52:25)
Copyright (c) The PHP Group
Zend Engine v4.1.0-dev, Copyright (c) Zend Technologies


在启动服务之前，需要修改 php-fpm.conf 配置文件，确保 php-fpm 模块使用 www-data 用户和 www-data 用户组的身份运行。

vim /usr/local/etc/php-fpm.d/www.conf
找到以下内容并修改：

; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
user = www-data
group = www-data


/usr/local/bin/php-fpm
/usr/local/bin/php-fpm
[23-Jun-2021 18:33:46] ERROR: unable to bind listening socket for address '127.0.0.1:9000': Address already in use (98)



### 创建配置文件，并将其复制到正确的位置。


cp php.ini-development /usr/local/php/php.ini

cp /usr/local/etc/php-fpm.d/www.conf.default /usr/local/etc/php-fpm.d/www.conf

cp sapi/fpm/php-fpm.conf /usr/local/etc/php-fpm.conf

cp sapi/fpm/php-fpm /usr/local/bin



 /usr/local/bin/php-fpm
[23-Jun-2021 18:24:46] ERROR: failed to open configuration file '/usr/local/etc/php-fpm.conf': No such file or directory (2)
[23-Jun-2021 18:24:46] ERROR: failed to load configuration file '/usr/local/etc/php-fpm.conf'

cp ./sapi/fpm/php-fpm.conf /usr/local/etc/php-fpm.conf



mkdir -p /usr/local/webserver/php8.1.0


```sh
$ ./configure --prefix=/usr/local/webserver/php8.1.0 \
--with-config-file-path=/usr/local/webserver/php8.1.0/etc \
--with-zlib-dir \
--with-freetype-dir \
--enable-mbstring \
--with-libxml-dir=/usr \
--enable-soap \
--enable-calendar \
--with-curl \
--with-mcrypt \
--with-gd \
--disable-rpath \
--enable-inline-optimization \
--with-bz2 \
--with-zlib \
--enable-sockets \
--enable-sysvsem \
--enable-sysvshm \
--enable-pcntl \
--enable-mbregex \
--enable-exif \
--enable-bcmath \
--with-mhash \
--enable-zip \
--with-pcre-regex \
--with-pdo-mysql \
--with-mysqli \
--with-mysql-sock=/var/run/mysqld/mysqld.sock \
--with-jpeg-dir=/usr \
--with-png-dir=/usr \
--enable-gd-native-ttf \
--with-openssl \
--with-fpm-user=www-data \
--with-fpm-group=www-data \
--with-imap \
--with-imap-ssl \
--with-kerberos \
--with-gettext \
--with-xmlrpc \
--with-xsl \
--enable-opcache \
--enable-fpm
```