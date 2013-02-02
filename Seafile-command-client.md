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
Initialize config file in a given path.

    usage: seaf-cli init -c <config> [-n <username>]

start
-----
Start seafile client

    usage: seaf-cli start -c <config> [-w <worktree>]

start-ccnet
-----------
Start ccnet daemon

    usage: seaf-cli start-ccnet -c <config>

start-seafile
-------------
Start seafile daemon

    usage: seaf-cli start-seafile -c <config> [-w <worktree>]

clone
-----
clone a repo from seafile server

    usage: seaf-cli clone -c <config> -r <repo-id> -u <seahub-server-url>
                          [-w <worktree> -n <username> -p <password>]

sync
----
Synchronize a repo from seafile server

    usage: seaf-cli sync -c <config> -r <repo-id>

desync
------
Desynchronize a repo from seafile server

    usage: seaf-cli desync -c <config> -r <repo-id>