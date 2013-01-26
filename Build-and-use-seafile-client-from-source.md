## Build Seafile Client ##

### Linux ###

#### Preparation ####

The following list is what you need to install on your development machine. __You should install all of them before you build seafile__.

Package names are according to Ubuntu 12.04. For other Linux distros, please find their corresponding names yourself.

* libevent-dev ( 2.0 or later ) (libevent-devel for Fedora)
* libcurl4-openssl-dev  (1.0.0 or later) (openssl-devel for Fedora)
* libgtk2.0-dev ( 2.24 or later) (gtk2-devel for Fedora)
* uuid-dev  (libuuid-devel for Fedora)
* intltool ( 0.40 or later) 
* libsqlite3-dev (3.7 or later)  (sqlite-devel for Fedora)
* python-mako 
* python-webpy
* python-simplejson
* libnotify-dev (libnotify-devel for Fedora)
* libappindicator-dev (needed for Ubuntu Unity Desktop, not requied otherwise)

#### Building ####

To build Seafile client, you need first build the latest version of **libsearpc** and **ccnet**.

##### libsearpc #####

* wget http://seafile.com.cn/downloads/libsearpc-latest.tar.gz
* cd libsearpc-${VERSION}
* ./configure --prefix=/usr
* make
* make install

##### ccnet #####

* wget http://seafile.com.cn/downloads/ccnet-latest.tar.gz
* cd ccnet-${VERSION}
* ./configure --prefix=/usr   ### `export PKG_CONFIG_PATH=/usr/lib/pkgconfig` if libsearpc is not found
* make
* make install

##### seafile #####

* wget http://seafile.com.cn/downloads/seafile-latest.tar.gz
* cd seafile-${VERSION}
* ./configure --prefix=/usr ### add `--enable-appindicator` if you use ubuntu
* make
* make install

Appindicator is needed for Unity desktop environment.

## Use Seafile Client ##

After you build and install Seafile client on Linux, you can start it by the `seafile-applet` command
```sh
   $ sudo ldconfig  ### (it is need sometimes after compilation)
   $ seafile-applet
```