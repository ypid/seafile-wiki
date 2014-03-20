# Specifies untracked files to ignore

## Description
A `seafile-ignore.txt` file specifies untracked files that Seafile should ignore.  Each line in a `seafile-ignore.txt` file specifies a pattern. You have to put `seafile-ignore.txt` in the root directory of a library. The following pattern format can be accepted.

1. A blank line matches no files.

1. A line starting with # serves as a comment.

1. Seafile supports wildcards in the pattern.  For example, "foo/*" matches "foo/1" and "foo/hello".  "foo/?" matches "foo/1" but not "foo/hello". Note that the wildcard character * matches all the paths under a directory. For instance, "foo/*.html" matches "foo/a.html" and "foo/templates/b.html".

1. If the pattern ends with a slash, it would **only** match a directory.  In other words, foo/ will match a directory foo and paths underneath it, but will not match a regular file or a symbolic link foo.

1. If a pattern doesn't end with a slash or a wildcard, it would **not** match a directory. For example, "foo" can only match regular file "foo" or a symbolic link; while "foo/" and "foo*" match a directory and paths under that directory.

## Notes
* The `seafile-ignore.txt` file only controls which files to ignore on the client side. You can still create a file from seahub web interface that's ignored on the client. In this case:
    - The created file will still be synced back to clients. But any later local changes to those files            will be ignored.
    - If the file is modified on seahub, the new version will also be synced back to clients; If the file on the client is also modified, a conflict file will be generated on the client.

* `seafile-ignore.txt` only ignores untracked files. If a file is already synced, and some time later you add it to the ignore list, its existing versions won't be removed.

* At the present seafile doesn't support global ignore file (it's been [proposed](https://github.com/haiwen/seafile/issues/561#issuecomment-37973230)). As a workaround you can create file `seafile-ignore.txt` in some directory (for example in root directory for library directories: `Seafile`) and create symlinks in all library directories to this file.

## Example

    # This is an example of Seafile ignore.txt file
    
    # a regular file
    test-file
    
    # a dir
    test-dir/
    
    # wildcard *
    test-star1/*
    test-star2/*.html
    
    # wildcard ?
    test-qu1/?.html
    test-qu2/?/
    
    # Ignore some common temporary/backup/OS-specific files
    *~
    *.bak
    .DS_Store
    ._*
    Thumbs.db
    Desktop.ini
    .directory
    *.swp
    *.kate-swp
