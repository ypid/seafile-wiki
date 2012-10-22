## Build Seafile Client ##

### Linux ###

#### Preparation ####

The following list is what you need to install on your development machine. __You should install all of them before you build seafile__.

Package names are according to Ubuntu 11.10. For other Linux distros, please find their corresponding names yourself.

* libevent-dev ( 2.0 or later )
* libcurl4-openssl-dev  (1.0.0 or later)
* libgtk-2.0 ( 2.28 or later)
* uuid-dev
* intltool ( 0.40 or later)
* libsqlite3-dev (3.7 or later)
* python2.7-dev
* python-mako
* python-webpy
* python-simplejson

#### Building ####

To build Seafile client, you need first build the latest version of **libsearpc** and **ccnet**.

##### libsearpc #####

* [download](https://www.github.com/haiwen/libsearpc/downloads) the latest version tarball , and untar it
* cd libsearpc-${VERSION}
* ./configure
* make
* make install

##### ccnet #####

* [download](https://www.github.com/haiwen/ccnet/downloads) the latest version tarball , and untar it
* cd ccnet-${VERSION}
* ./configure
* make
* make install

##### seafile #####

* [download](https://www.github.com/haiwen/seafile/downloads) the latest version tarball , and untar it
* cd seafile-${VERSION}
* ./configure
* make
* make install

### Windows ###

#### Install MinGW ####

On windows, The MinGW toolchain is used to compile Seafile. Download [mingw-get-install](http://sourceforge.net/projects/mingw/files/Automated%20MinGW%20Installer/mingw-get-inst) and install MinGW with it.

#### Get the libraries ####

Download the following libraries from [gtk-windows page](http://www.gtk.org/download/win32.php)

**Note**: For each library, both **run-time** and **dev** files should be downloaded.

+ glib 
+ zlib 
+ gettext-runtime 
+ intltool 
+ pkg-config 
+ gettext-tools

Unzip all these packages to c:/MinGW. 

##### Other libraries #####

+ [openssl](http://www.openssl.org/source/openssl-1.0.0d.tar.gz). Download the tarball and uncompress it, then open the MinGW shell and go to the uncompressed directory.

    $ ./configure --prefix=/mingw mingw shared

    $ make && make install

+ [libevent 2.11](http://monkey.org/~provos/libevent-2.0.11-stable.tar.gz).

    $ ./configure --prefix=/mingw;

    $ make && make install

+ [libsqlite3](http://www.sqlite.org/sqlite-autoconf-3070701.tar.gz). The same with libevent.

##### valac #####

Download [vala](http://vala-win32.googlecode.com/files/vala-0.12.0.exe) and install it. The installer would automaticaly add `C:\vala-0.12\bin` to PATH for you.

#### Python-related ####

+ [Python 2.6](http://www.python.org/ftp/python/2.6/python-2.6.msi)
+ Download [python26.dll](http://www.dll-files.com/python26.zip?0WGgUFcEmQ) and put it to `C:\Python26\libs`, assume you install python2.6 in `C:\Python26`.
+ [Py2exe 2.6](http://sourceforge.net/projects/py2exe/files/py2exe/0.6.9/py2exe-0.6.9.win32-py2.6.exe/download)
+ [easy_install](http://pypi.python.org/packages/2.6/s/setuptools/setuptools-0.6c11.win32-py2.6.exe#md5=1509752c3c2e64b5d0f9589aafe053dc)

After that, add `C:\Python26` and `C:\Python26\scripts` to `PATH` environment variable. Then install the following python packages with easy_install:

+ web.py
+ mako
+ simplejson

```sh
easy_install web.py mako simplejson
```

## Use Seafile Client ##

### Linux ###

After you build and install Seafile client on Linux, you can start it by the `seafile-applet command`
```sh
   $ seafile-applet
```

### Windows ###

* After download the installer(a .msi file), click the msi file and follow the installer to finish the installation.
* The program will start automatically after installation.