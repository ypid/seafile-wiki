
== System Admin ==

=== Create a non-root user ===
<pre>
useradd -d /home/plt -m -p hello123 -s /bin/bash plt
/usr/sbin/visudo
</pre>

=== Add EPEL to yum repository ===

[http://fedoraproject.org/wiki/EPEL EPEL]: Extra Packages for Enterprise Linux 

<pre>
cd /tmp
wget http://mirror.nus.edu.sg/Fedora/epel/5/i386/epel-release-5-4.noarch.rpm
sudo rpm -ivh epel-release-5-4.noarch.rpm
</pre>

=== install packages ===

<pre>
sudo yum install python26-dev gcc
</pre>


== Install library dependencies ==

To seperate system libraries from the libraries we build, we install it to '''/opt'''. Later we can link to the libraries we build by simply set the @PKT_CONFIG_PATH@ environment variable.
<pre>
export PKG_CONFIG_PATH=/opt/lib/pkgconfig
export LD_LIBRARY_PATH=/opt/lib
</pre>

=== GLib ===

# download latest from [http://developer.gnome.org/glib/]
# build and install

<pre>
./configure --prefix=/opt
make
make install
</pre>

