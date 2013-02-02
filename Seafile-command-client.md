Overview
========

Seaf-cli is used to run seafile client in command line.  The first command you need to run is 'init' command, which creates config files for you. Then you can start seafile-client and clone libraries from servers.


Subcommands
===========
    init:           initialize config file before starting seafile client
    start:          start seafile client
    stop:           stop seafile client
    clone:          clone a library from seafile server
    sync:           synchronize an existing folder with a library from seafile server
    desync:         desynchronize a folder with seafile server


Usage
=====

init
----
Initialize seafile client. This command create sub-directories `seafile-data` and `seafile` under `parent-dir`. 

    usage: seaf-cli init -p <parent-dir>

start
-----
Start seafile client. This command start `ccnet` and `seaf-daemon`, `ccnet` is the network part of seafile client, `seaf-daemon` manages the files.

    usage: seaf-cli start

stop
----

    usage: seaf-cli stop

clone
-----
clone a library from seafile server

    usage: seaf-cli clone -r <library-id> -h <seahub-server-url> -u <username> -p <password>

sync
----
Synchronize a library with an existing folder

    usage: seaf-cli sync -r <library-id> -h <seahub-server-url> -d <existing-folder> -u <username> -p <password>

desync
------
Desynchronize a library from seafile server

    usage: seaf-cli desync -d <existing-folder>
