Overview
========

Seaf-cli.py program is used to run seafile client in command line.  The first
command you need to run is 'init' command, which creates some config files for
you, when you are the first time to use seafile client.  If you already have a
config file, you can run other commands directly.

After you already create a config file, you can use other commands.  'start'
command runs seafile-applet to run seafile client.  'start-ccnet' only run
ccnet daemon for you in background, and 'start-seafile' only run seafile daemon
in background.  You need to start a ccnet daemon before starting a seafile
daemon.  'clone' command will clone a repo you indicated.  'sync' command will
try to synchronize a repo that has be cloned, and 'remove' will make a repo
not be synchronized.


Commands
========

init
----
Initialize config file

usage: seaf-cli.py -c <config-dir> -o init

start
-----
Start seafile-applet to run a seafile client

usage: seaf-cli.py -c <config-dir> -o start

start-ccnet
-----------
Start ccnet daemon

usage: seaf-cli.py -c <config-dir> -o start-ccnet

start-seafile
-------------
Start seafile daemon

usage: seaf-cli.py -c <config-dir> [-w <worktree>] -o start-seafile

clone
-----
Clone a repo from seafile server

A repo id and a url need to be give because this program need to use seafile web
API v2 to fetch repo information.

usage: seaf-cli.py -c <config-dir> -r <repo-id> -u <url> [-w <worktree>] -o clone

sync
----
Try to synchronize a repo

usage: seaf-cli.py -c <config-dir> -r <repo-id> -o clone

remove
------
Try to desynchronize a repo

usage: seaf-cli.py -c <config-dir> -r <repo-id> -o remove