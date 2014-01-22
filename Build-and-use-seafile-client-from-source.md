## Build Seafile Client ##

### The components of Seafile client ###

Seafile client v2.0 consists of three components:

* ccnet-daemon: for networking
* seafile-daemon: for file syncing
* seafile-applet: GUI

### Linux ###

#### Preparation ####

The following list is what you need to install on your development machine. __You should install all of them before you build seafile__.

Package names are according to Ubuntu 12.04. For other Linux distros, please find their corresponding names yourself.

* autoconf/automake/libtool
* libevent-dev ( 2.0 or later )
* libcurl4-openssl-dev  (1.0.0 or later)
* libgtk2.0-dev ( 2.24 or later)
* uuid-dev
* intltool ( 0.40 or later) 
* libsqlite3-dev (3.7 or later)
* valac  (only needed if you build from git repo)
* libjansson-dev
* libqt4-dev
* valac
* cmake
* libfuse-dev (for seafile >= 2.1)

```bash
sudo apt-get install autoconf automake libtool libevent-dev libcurl4-openssl-dev libgtk2.0-dev uuid-dev intltool libsqlite3-dev valac libjansson-dev libqt4-dev cmake libfuse-dev
```
For a fresh Fedora 19 installation, the following will install all dependencies via YUM:

```bash
$ sudo yum install wget gcc libevent-devel openssl-devel gtk2-devel libuuid-devel sqlite-devel jansson-devel intltool cmake qt-devel fuse-devel
```

#### Building ####

First you should get the latest source of libsearpc/ccnet/seafile/seafile-client:

Download the source tarball of the latest tag from 

- https://github.com/haiwen/libsearpc/tags
- https://github.com/haiwen/ccnet/tags
- https://github.com/haiwen/seafile/tags
- https://github.com/haiwen/seafile-client/tags

For example, if the latest released seafile client is 2.0.8, then just use the **v2.0.8** tags of the four projects. You should get four tarballs:

- libsearpc-2.0.8.tar.gz
- ccnet-2.0.8.tar.gz
- seafile-2.0.8.tar.gz
- seafile-client-2.0.8.tar.gz

```sh
export version=2.0.8
alias wget='wget --content-disposition -nc'
wget https://github.com/haiwen/libsearpc/archive/v${version}.tar.gz
wget https://github.com/haiwen/ccnet/archive/v${version}.tar.gz
wget https://github.com/haiwen/seafile/archive/v${version}.tar.gz
wget https://github.com/haiwen/seafile-client/archive/v${version}.tar.gz
```

Now uncompress them:

```sh
tar xf libsearpc-${version}.tar.gz
tar xf ccnet-${version}.tar.gz
tar xf seafile-${version}.tar.gz
tar xf seafile-client-${version}.tar.gz
```

To build Seafile client, you need first build **libsearpc** and **ccnet**, **seafile**.

##### libsearpc #####

```bash
cd libsearpc-${version}
./autogen.sh
./configure --prefix=/usr
make
sudo make install
```

##### ccnet #####

NOTE: If libsearpc is not found while executing `configure` below, run: `export PKG_CONFIG_PATH=/usr/lib/pkgconfig`.

```bash
cd ccnet-${version}
./autogen.sh
./configure --prefix=/usr
make
sudo make install
```

##### seafile #####

```bash
cd seafile-${version}/
./autogen.sh
./configure --prefix=/usr --disable-gui
make
sudo make install
```

#### seafile-client ####

```bash
cd seafile-client-${version}
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr .
make
sudo make install
```

## Use Seafile Client ##

After you build and install Seafile client on Linux, you can start it by the `seafile-applet` command
```sh
   $ sudo ldconfig  ### (it is need sometimes after compilation)
   $ seafile-applet
```