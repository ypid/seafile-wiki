## Build Seafile Client ##

### The components of seafile client ###

[[images/client-arch.png]]

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
* valac  (only needed if you build from git repo)
* libjansson-dev
* libqt4-dev
* cmake

For a fresh Fedora 19 installation, the following will install all dependencies via YUM:

```bash
$ sudo yum install wget gcc libevent-devel openssl-devel gtk2-devel libuuid-devel sqlite-devel jansson-devel intltool cmake qt-devel
```

#### Building ####

First you should get the latest source of seafile:

```bash
$ wget http://seafile.googlecode.com/files/seafile-latest.tar.gz
$ tar xzf seafile-latest.tar.gz
```

After running the above commands, you should have a folder `seafile-{VERSION}` (VERSION is the latest version number, for example 1.4.5) in your current directory. The source of libsearpc/ccnet/seafile-client is also included in that folder.

```bash
seafile-{VERSION}
├── libsearpc
├── ccnet
├── seafile-client
├── ... (other files)
```

To build Seafile client, you need first build **libsearpc** and **ccnet**, **seafile**.

##### libsearpc #####

```bash
$ cd seafile-{VERSION}/libsearpc
$ ./configure --prefix=/usr
$ make
$ sudo make install
```

##### ccnet #####

NOTE: If libsearpc is not found while executing `configure` below, run: `export PKG_CONFIG_PATH=/usr/lib/pkgconfig`.

```bash
$ cd seafile-{VERSION}/ccnet
$ ./configure --prefix=/usr
$ make
$ sudo make install
```

##### seafile #####

```bash
$ cd seafile-${VERSION}/
$ ./configure --prefix=/usr --disable-gui
$ make
$ sudo make install
```

#### seafile-client ####

```bash
$ cd seafile-${VERSION}/seafile-client
$ cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr .
$ make
$ sudo make install
```

## Use Seafile Client ##

After you build and install Seafile client on Linux, you can start it by the `seafile-applet` command
```sh
   $ sudo ldconfig  ### (it is need sometimes after compilation)
   $ seafile-applet
```
