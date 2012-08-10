## HOWTO build a debian package for seafile client

1. Prepare
Before build package, you need to compile 'libsearpc' and 'ccnet' libraries because seafile depends on them.

* Build libsearpc

> wget https://github.com/haiwen/libsearpc/zipball/master

> tar zxf libsearpc.tar.gz

> mv libsearpc deb-libsearpc

> cd deb-libsearpc

> ./autogen.sh

> ./configure --prefix=/usr

> make

> make install

* Build ccnet

> wget https://github.com/haiwen/ccnet/zipball/master

> tar zxf ccnet.tar.gz

> mv ccnet deb-ccnet

> cd deb-ccnet

> ./autogen.sh

> ./configure --prefix=/usr

> make

> make install

1. Build debian package