## Install MinGW

On windows, The MinGW toolchain is used to compile Seafile. Download [mingw-get-install](http://sourceforge.net/projects/mingw/files/Automated%20MinGW%20Installer/mingw-get-inst) and install MinGW with it.

## Get the libraries 

Download the following libraries from [gtk-windows page](http://www.gtk.org/download/win32.php)

**Note**: For each library, both **run-time** and **dev** files should be downloaded.

+ glib 
+ zlib 
+ gettext-runtime 
+ intltool 
+ pkg-config 
+ gettext-tools

Unzip all these packages to c:/MinGW. 

### Other libraries

+ [openssl](http://www.openssl.org/source/openssl-1.0.0d.tar.gz). Download the tarball and uncompress it, then open the MinGW shell and go to the uncompressed directory.

    $ ./configure --prefix=/mingw mingw shared

    $ make && make install

+ [libevent 2.11](http://monkey.org/~provos/libevent-2.0.11-stable.tar.gz).

    $ ./configure --prefix=/mingw;

    $ make && make install

+ [libsqlite3](http://www.sqlite.org/sqlite-autoconf-3070701.tar.gz). The same with libevent.

### valac

Download [vala](http://vala-win32.googlecode.com/files/vala-0.12.0.exe) and install it. The installer would automaticaly add `C:\vala-0.12\bin` to PATH for you.

## Python-related

+ [Python 2.6](http://www.python.org/ftp/python/2.6/python-2.6.msi)
+ Download [python26.dll](http://www.dll-files.com/python26.zip?0WGgUFcEmQ) and put it to `C:\Python26\libs`, assume you install python2.6 in `C:\Python26`.
+ [Py2exe 2.6](http://sourceforge.net/projects/py2exe/files/py2exe/0.6.9/py2exe-0.6.9.win32-py2.6.exe/download)
+ [easy_install](http://pypi.python.org/packages/2.6/s/setuptools/setuptools-0.6c11.win32-py2.6.exe#md5=1509752c3c2e64b5d0f9589aafe053dc)

After that, add `C:\Python26` and `C:\Python26\scripts` to `PATH` environment variable. Then install the following python packages with easy_install:

+ web.py
+ mako
+ simplejson

    $ easy_install web.py mako simplejson

