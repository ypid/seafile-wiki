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
* python-mako 
* python-webpy
* python-simplejson
* libnotify-dev
* libappindicator-dev (needed for Ubuntu Unity Desktop, not requied otherwise)
* valac  (only needed if you build from git repo)

For Fedora, they are libevent-devel, openssl-devel, gtk2-devel, libuuid-devel, sqlite-devel, libnotify-devel.

#### Building ####

First you should get the latest source of seafile:

```bash
wget http://seafile.com.cn/downloads/seafile-latest.tar.gz
tar xzf seafile-latest.tar.gz
```

After running the above commands, you should have a folder `seafile-{VERSION}` (VERSION is the latest version number, for example 1.4.5) in your current directory. The source of libsearpc/ccnet is also included in that folder.

```bash
seafile-{VERSION}
├── libsearpc
├── ccnet
├── ... (other files)
```

To build Seafile client, you need first build **libsearpc** and **ccnet**. 

##### libsearpc #####

```bash
cd seafile-{VERSION}/libsearpc
./configure --prefix=/usr
make
make install
```

##### ccnet #####

```bash
cd seafile-{VERSION}/ccnet
./configure --prefix=/usr   ### `export PKG_CONFIG_PATH=/usr/lib/pkgconfig` if libsearpc is not found
make
make install
```

##### seafile #####

```bash
cd seafile-${VERSION}/
./configure --prefix=/usr ### add `--enable-appindicator` if you use ubuntu
make
make install
```

Appindicator is needed for Unity desktop environment.

## Use Seafile Client ##

After you build and install Seafile client on Linux, you can start it by the `seafile-applet` command
```sh
   $ sudo ldconfig  ### (it is need sometimes after compilation)
   $ seafile-applet
```