seaf-cli is command line interface for seafile client.

Subcommands:

    init:           create config files for seafile client
    start:          start and run seafile client as daemon
    stop:           stop seafile client
    list:           list local liraries
    status:         show syncing status
    download:       download a library from seafile server
    sync:           synchronize an existing folder with a library in 
                        seafile server
    desync:         desynchronize a library with seafile server


Detail
======

Seafile client stores all its configure information in a config dir. The default location is `~/.ccnet`. All the commands below accept an option `-c <config-dir>`.

init
----
Initialize seafile client. This command initializes the config dir. It also creates sub-directories `seafile-data` and `seafile` under `parent-dir`. `seafile-data` is used to store internal data, while `seafile` is used as the default location put downloaded libraries. 

    seaf-cli init [-c <config-dir>] -d <parent-dir>

start
-----
Start seafile client. This command start `ccnet` and `seaf-daemon`, `ccnet` is the network part of seafile client, `seaf-daemon` manages the files.

    seaf-cli start [-c <config-dir>]

stop
----
Stop seafile client.

    seaf-cli stop [-c <config-dir>]


clone
-----
clone a library from seafile server

    seaf-cli clone -l <library-id> -h <seahub-server-url> -d <parent-directory> -u <username> -p <password>


sync
----
Synchronize a library with an existing folder.

    seaf-cli sync -l <library-id> -h <seahub-server-url> -d <existing-folder> -u <username> -p <password>

desync
------
Desynchronize a library from seafile server

    seaf-cli desync -d <existing-folder>
