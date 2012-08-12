## Install MinGW

On windows, The MinGW toolchain is used to compile Seafile. Download [mingw-get-install](http://sourceforge.net/projects/mingw/files/Automated%20MinGW%20Installer/mingw-get-inst) and install MinGW with it.

## Get the libraries 

Download the following libraries from [gtk-windows page](http://www.gtk.org/download/win32.php)

*note*: both run-time and dev files should be downloaded.

+ glib 
+ zlib 
+ gettext-runtime 
+ intltool 
+ pkg-config 
+ gettext-tools

Unzip all these packages to c:/MinGW. 

## Python-related

+ [Python 2.6](http://www.python.org/ftp/python/2.6/python-2.6.msi)
+ [Py2exe 2.6](http://sourceforge.net/projects/py2exe/files/py2exe/0.6.9/py2exe-0.6.9.win32-py2.6.exe/download)
+ [easy_install](http://pypi.python.org/packages/2.6/s/setuptools/setuptools-0.6c11.win32-py2.6.exe#md5=1509752c3c2e64b5d0f9589aafe053dc)

After that, add `C:\Python26\scripts` and `C:\Python26` to `PATH` environment variable. Then install the following python packages with easy_install:

+ web.py
+ mako
+ simplejson