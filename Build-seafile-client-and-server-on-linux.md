## Prerequiste

To build Seafile, you need first build and install:

* [libsearpc](https://github.com/haiwen/libsearpc) 
* [ccnet](https://www.github.com/haiwen/ccnet)

The following list is what you need to install on your development machine. __You should install all of them before you try to compile any of libsearpc/ccnet/seafile__.

(Package names are according to Ubuntu 11.10. For other Linux distros, please find their corresponding names yourself)

### Require by both client/server

* libevent-dev
* libcurl4-openssl-dev
* libgtk-3-dev
* uuid-dev
* intltool 
* libsqlite3-dev
* valac
* python2.7-dev 

### Required by client
* libnotify-dev
* libappindicator-dev
* python-webpy

### Required by server
* libmysqlclient-dev
* libzdb (http://www.tildeslash.com/libzdb/dist/libzdb-2.10.5.tar.gz)
* libevhtp (https://github.com/downloads/ellzey/libevhtp/libevhtp-0.3.0.tar.gz)

libzdb relies on two packages: `re2c` and `flex`  
libevhtp needs to be built by `cmake`


### Building Client ###
* download tarball and untar this file
* cd seafile-${VERSION}
* ./configure
* make
* make install

### Building Server ###
* download tarball and untar this file
* cd seafile-${VERSION}
* ./configure --enable-server --enable-httpserver
* make
* make install

### Problems Report

If you encounter any problem when building Seafile, please leave us a message or open an issue.