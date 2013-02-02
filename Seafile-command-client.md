Overview
========

Seaf-cli.py program is used to run seafile client in command line.  The first
command you need to run is 'init' command, which creates some config files for
you, when you are the first time to use seafile client.  If you already have a
config file, you can run other commands directly.


Subcommands
===========
    init:           initialize config file before starting seafile client
    start:          start seafile client
    start-ccnet:    start ccnet daemon
    start-seafile:  start seafile daemon
    clone:          clone a repo from seafile server
    sync:           synchronize a repo from seafile server
    desync:         desynchronize a repo from seafile server


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
