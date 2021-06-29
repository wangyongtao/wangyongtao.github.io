

PPA 表示 个人软件包存档(Personal Package Archive)。

每个版本的 Ubuntu 都有自己的四个官方软件仓库：

Main - Canonical 支持的自由开源软件。
Universe - 社区维护的自由开源软件。
Restricted - 设备的专有驱动程序。
Multiverse - 受版权或法律问题限制的软件。


当某个软件发布了新版本，Ubuntu 很大可能不会立即提供该新版本的软件。Ubuntu 提供了一个名为 Launchpad 的平台，使软件开发人员能够创建自己的软件仓库。

终端用户，也就是你，可以将 PPA 仓库添加到 sources.list 文件中，当你更新系统时，你的系统会知道这个新软件的可用性，然后你可以使用标准的 sudo apt install 命令安装它。


目前仅能安装 php7.4

```
$ php -v

Command 'php' not found, but can be installed with:
apt install php7.4-cli
```



Debian开发人员Ondrej Sury维护着一个包含多个PHP版本的存储库


```sh
$ sudo apt install software-properties-common

$ sudo add-apt-repository ppa:ondrej/php
```





```sh
$ sudo apt install php8.0             

Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  apache2 apache2-bin apache2-data 
  apache2-utils libapache2-mod-php8.0 
  libapr1 libaprutil1 libaprutil1-dbd-sqlite3 
  libaprutil1-ldap libjansson4 
  php-common php8.0-cli
  php8.0-common php8.0-opcache 
  php8.0-readline ssl-cert
Suggested packages:
  apache2-doc apache2-suexec-pristine | apache2-suexec-custom 
  www-browser 
  php-pear 
  openssl-blacklist
The following NEW packages will be installed:
  apache2 apache2-bin apache2-data apache2-utils libapache2-mod-php8.0 
  libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap 
  libjansson4 php-common php8.0 php8.0-cli
  php8.0-common php8.0-opcache php8.0-readline ssl-cert
0 upgraded, 17 newly installed, 0 to remove and 213 not upgraded.
Need to get 6,190 kB of archives.
After this operation, 27.8 MB of additional disk space will be used.
Do you want to continue? [Y/n] 
```


```sh
$ apt search php8.0

libapache2-mod-php8.0: server-side, HTML-embedded scripting language (Apache 2 module)
libphp8.0-embed: HTML-embedded scripting language (Embedded SAPI library)
php8.0: server-side, HTML-embedded scripting language (metapackage)
php8.0-amqp: AMQP extension for PHP
php8.0-apcu: APC User Cache for PHP
php8.0-ast: AST extension for PHP 7
php8.0-bcmath: Bcmath module for PHP
php8.0-bz2: bzip2 module for PHP
php8.0-cgi: server-side, HTML-embedded scripting language (CGI binary)
php8.0-cli: command-line interpreter for the PHP scripting language
php8.0-common: documentation, examples and common module for PHP
php8.0-curl: CURL module for PHP
php8.0-dba: DBA module for PHP
php8.0-decimal: Arbitrary precision floating-point decimal for PHP
php8.0-dev: Files for PHP8.0 module development
php8.0-ds: PHP extension providing efficient data structures for PHP 7
php8.0-enchant: Enchant module for PHP
php8.0-fpm: server-side, HTML-embedded scripting language (FPM-CGI binary)
php8.0-gd: GD module for PHP
php8.0-gearman: PHP wrapper to libgearman
php8.0-gmagick: Provides a wrapper to the GraphicsMagick library
php8.0-gmp: GMP module for PHP
php8.0-gnupg: PHP wrapper around the gpgme library
php8.0-grpc: High performance, open source, general RPC framework for PHP
php8.0-http: PECL HTTP module for PHP Extended HTTP Support
php8.0-igbinary: igbinary PHP serializer
php8.0-imagick: Provides a wrapper to the ImageMagick library
php8.0-imap: IMAP module for PHP
php8.0-inotify: Inotify bindings for PHP
php8.0-interbase: Interbase module for PHP
php8.0-intl: Internationalisation module for PHP
php8.0-ldap: LDAP module for PHP
php8.0-lz4: LZ4 Extension for PHP
php8.0-mailparse: Email message manipulation for PHP
php8.0-maxminddb: Reader for the MaxMind DB file format for PHP
php8.0-mbstring: MBSTRING module for PHP
php8.0-mcrypt: PHP bindings for the libmcrypt library
php8.0-memcache: memcache extension module for PHP
php8.0-memcached: memcached extension module for PHP, uses libmemcached
php8.0-mongodb: MongoDB driver for PHP
php8.0-msgpack: PHP extension for interfacing with MessagePack
php8.0-mysql: MySQL module for PHP
php8.0-oauth: OAuth 1.0 consumer and provider extension
php8.0-odbc: ODBC module for PHP
php8.0-opcache: Zend OpCache module for PHP
php8.0-pcov: Code coverage driver
php8.0-pgsql: PostgreSQL module for PHP
php8.0-phpdbg: server-side, HTML-embedded scripting language (PHPDBG binary)
php8.0-protobuf: Protocol buffers bindings for PHP
php8.0-pspell: pspell module for PHP
php8.0-psr: PSR interfaces for PHP
php8.0-raphf: raphf module for PHP
php8.0-readline: readline module for PHP
php8.0-redis: PHP extension for interfacing with Redis
php8.0-rrd: PHP bindings to rrd tool system
php8.0-smbclient: PHP wrapper for libsmbclient
php8.0-snmp: SNMP module for PHP
php8.0-soap: SOAP module for PHP
php8.0-solr: PHP extension for communicating with Apache Solr server
php8.0-sqlite3: SQLite3 module for PHP
php8.0-ssh2: Bindings for the libssh2 library
php8.0-swoole: Swoole Coroutine Fiber Async Programming Framework for PHP
php8.0-sybase: Sybase module for PHP
php8.0-tidy: tidy module for PHP
php8.0-uuid: PHP UUID extension
php8.0-vips: PHP extension for interfacing with libvips
php8.0-xdebug: Xdebug Module for PHP
php8.0-xhprof: Hierarchical Profiler for PHP 5.x
php8.0-xml: DOM, SimpleXML, XML, and XSL module for PHP
php8.0-xmlrpc: XML-RPC servers and clients functions for PHP
php8.0-xsl:XSL module for PHP (dummy)
php8.0-yac: YAC (Yet Another Cache) for PHP
php8.0-yaml: YAML-1.1 parser and emitter for PHP
php8.0-zip: Zip module for PHP
php8.0-zmq: ZeroMQ messaging bindings for PHP
php8.0-zstd: Zstandard extension for PHP
```

查看已经加载的PHP模块： 

```sh
$ php -m
[PHP Modules]
calendar
Core
ctype
date
exif
FFI
fileinfo
filter
ftp
gettext
hash
iconv
json
libxml
openssl
pcntl
pcre
PDO
Phar
posix
readline
Reflection
session
shmop
sockets
sodium
SPL
standard
sysvmsg
sysvsem
sysvshm
tokenizer
Zend OPcache
zlib

[Zend Modules]
Zend OPcache
```


sudo apt install php8.0-cli php8.0-opcache php8.0-fpm php8.0-mysql php8.0-redis


安装更多有用扩展的示例：

sudo apt install php8.0-{curl,mysql,redis,xml,bz2,curl,intl,readline}


```sh
$ php -v
PHP 8.0.7 (cli) (built: Jun  4 2021 21:26:10) ( NTS )
Copyright (c) The PHP Group
Zend Engine v4.0.7, Copyright (c) Zend Technologies
  with Zend OPcache v8.0.7, Copyright (c), by Zend Technologies
```
