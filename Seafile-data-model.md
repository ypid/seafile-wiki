Seafile uses a data model like GIT. It consists of `Repo`, `Branch`, `Commit`, `FS`, and `Block`.

## Repo

A repo is also called library. Every repo has an unique id (UUID), and attributes like description, creator, password.

## Branch

Unlike git, only two predefined branches is used now, i.e., `local` and `master`. 

In PC client, modifications will first be committed to local branch. Then the master branch is downloaded from server, and merged into local branch. Then the local branch will be uploaded to server and replace the master branch there.

In Web API, modifications will first be committed to local on server, then merged into the master.

## Commit

Like in GIT.

## FS

There are two types of FS objects, `SeafDir Object` and `Seafile Object`.
`SeafDir Object` represents a directory, and `Seafile Object` represents a file.

## Block

A file is breaked into blocks, which is typically of size 1MB. This mechanism is for easy storing and transfering of files.
 