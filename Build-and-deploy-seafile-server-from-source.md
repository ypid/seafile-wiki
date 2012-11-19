## Build Seafile Server ##

**Warning: not finished yet!**

### Preparation ###

The following list is what you need to install on your development machine. __You should install all of them before you build seafile__.

Package names are according to Ubuntu 12.04. For other Linux distros, please find their corresponding names yourself.

* libevent-dev ( 2.0 or later )
* libcurl4-openssl-dev  (1.0.0 or later)
* libglib2.0-dev ( 2.28 or later)
* uuid-dev
* intltool ( 0.40 or later)
* libsqlite3-dev (3.7 or later)
* python2.7-dev 
* libmysqlclient-dev (5.5 or later)

The following libraries need to be compiled from source.

* libzdb (http://www.tildeslash.com/libzdb/dist/libzdb-2.10.5.tar.gz)
* libevhtp (https://github.com/ellzey/libevhtp/archive/1.1.6.zip)

libzdb relies on two packages: `re2c` and `flex`  
libevhtp needs to be built by `cmake`

### Building ###

To build Seafile Server, you need first build the latest version of **libsearpc** and **ccnet**.

#### libsearpc ####

* [download libsearpc-1.0.1](https://github.com/downloads/haiwen/libsearpc/libsearpc-1.0.1.tar.gz), and untar it
* cd libsearpc-1.0.1
* ./configure
* make
* make install

#### ccnet-server ####

* [download ccnet-1.0.1](https://github.com/downloads/haiwen/ccnet/ccnet-1.0.1.tar.gz), and untar it
* cd ccnet-1.0.1
* ./configure --disable-client --enable-server
* make
* make install

#### seafile ####

* [download seafile-1.2.0](https://github.com/downloads/haiwen/seafile/seafile-1.2.0.tar.gz), and untar it
* cd seafile-1.2.0
* ./configure --disable-client --enable-server --enable-httpserver
* make
* make install

## Deploy Seafile Server ##

To deploy Seafile server, we need first know about its components.

### Components of the Seafile server

The Seafile server consists of the following parts:

<table>
  <tr>
    <th>Process Name</th><th>Functionality</th>
  </tr>
  <tr>
    <td>ccnet-server</td><td>underlying networking</td>
  </tr>
  <tr>
    <td>seaf-server</td><td>data management</td>
  </tr>
  <tr>
    <td>seaf-monitor</td><td>monitoring seafile statistics</td>
  </tr>
  <tr>
    <td>Seahub</td><td>website front-end of Seafile server</td>
  </tr>
  <tr>
    <td>httpserver</td><td>handles raw file upload/download for Seahub</td>
  </tr>
</table>

Seahub is the web front-end of Seafile. It's written in the [Django](http://djangoproject.com) framework.
Seahub requires Python 2.7 installed on your server, and it depends on the following python libraries:  

* [django 1.3](https://www.djangoproject.com/download/1.3.1/tarball/)
* [djblets](https://github.com/djblets/djblets/tarball/release-0.6.14)
* sqlite3
* simplejson
* PIL (aka. python imaging library)
* gunicorn

Before continue, make sure you have all these libraries available in your system.

### Create Configuration ###

To create the configurations, you can choose either:

* use the seafile-admin script (Recommended)
* [[create server configuration by hand]]

#### Create Configurations with the seafile-admin script ####

To help you create the configuration, Seafile includes a script called **seafile-admin**, which should have been installed to system path after you have built and installed Seafile from source.

To use the script:

* Create a new directory for all the configuration and data
* Enter the directory, and download seahub to this directory.
* Run `seafile-admin setup` to create all the configuration
* After configuration successfully created, run `seafile-admin start` to start the all components of Seafile.
* When needed, run `seafile-admin stop` to stop all components of Seafile.

```sh
mkdir /data/abc-seafile
cd /data/abc-seafile
git clone git@github.com:haiwen/seahub.git
seafile-admin setup # it will guide you step by step
seafile-admin start # start all components
seafile-admin stop
```

## Problems Report ##

If you encounter any problem when building/deploying Seafile, please leave us a message or open an issue.