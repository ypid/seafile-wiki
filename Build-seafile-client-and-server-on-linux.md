## Prerequiste

To build Seafile, you need first build and install:

* [libsearpc](https://github.com/haiwen/libsearpc) 
* [ccnet](https://www.github.com/haiwen/ccnet)

The following list is what you need to install on your development machine. Once you have them installed, you can build all of libsearpc/ccnet/seafile.

(package names are according to Ubuntu 11.10, for other Linux distros, please find their corresponding names yourself)

### Require by both client/server

* build-essential 
* autoconf automake libtool
* libevent-dev
* libcurl4-openssl-dev
* libgtk-3-dev
* uuid-dev
* intltool 
* libsqlite3-dev
* valac
* python2.7-dev 
* python-webpy

### Required by client
* libnotify-dev
* libappindicator-dev

### Required by server
* libmysqlclient-dev
* libzdb (http://www.tildeslash.com/libzdb/dist/libzdb-2.10.5.tar.gz)
* libevhtp

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