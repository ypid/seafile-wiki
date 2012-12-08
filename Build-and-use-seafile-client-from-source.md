## Build Seafile Client ##

### Linux ###

#### Preparation ####

The following list is what you need to install on your development machine. __You should install all of them before you build seafile__.

Package names are according to Ubuntu 12.04. For other Linux distros, please find their corresponding names yourself.

* libevent-dev ( 2.0 or later )
* libcurl4-openssl-dev  (1.0.0 or later)
* libgtk2.0-dev ( 2.24 or later)
* uuid-dev
* intltool ( 0.40 or later)
* libsqlite3-dev (3.7 or later)
* python2.7-dev
* python-mako
* python-webpy
* python-simplejson
* libnotify-dev
* libappindicator-dev (needed for Ubuntu Unity Desktop, not requied otherwise)

#### Building ####

To build Seafile client, you need first build the latest version of **libsearpc** and **ccnet**.

##### libsearpc #####

* [download](https://www.github.com/haiwen/libsearpc/downloads) the latest version tarball , and untar it
* cd libsearpc-${VERSION}
* ./configure --prefix=/usr
* make
* make install

##### ccnet #####

* [download](https://www.github.com/haiwen/ccnet/downloads) the latest version tarball , and untar it
* cd ccnet-${VERSION}
* ./configure --prefix=/usr
* make
* make install

##### seafile #####

* [download](https://www.github.com/haiwen/seafile/downloads) the latest version tarball , and untar it
* cd seafile-${VERSION}
* ./configure --prefix=/usr --enable-appindicator
* make
* make install

Appindicator is needed for Unity desktop environment.


## Use Seafile Client ##

After you build and install Seafile client on Linux, you can start it by the `seafile-applet` command
```sh
   $ seafile-applet
```
