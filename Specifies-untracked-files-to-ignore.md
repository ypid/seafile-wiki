# Specifies untracked files to ignore

## Description
A ignore.txt file specifies untracked files that seafile should ignore.  Each line in a ignore.txt file specifies a pattern.  The following pattern format can be accepted.

1. A blank line matches no files.

1. A line starting with # serves as a comment.

1. If the pattern ends with a slash, it would only find a match iwth a directory.  In other words, foo/ will match a directory foo and paths underneath it, but will not match a regular file or a symbolic link foo.

1. seafile supports wildcards in the pattern.  For example, "foo/*" matches "foo/1" and "foo/hello".  "foo/?" matches "foo/1" but not "foo/hello".

## Notes
Although Seafile tries to do the best effect, you can always create a new file on web page, which a pattern matches.  So after such a file is created, the file still could be synchronized into local library.  Unfortunately, if you modify this file in local library after that, you will get a unmerged stage library, and that will cause Seafile couldn't synchronize your local library with remote library.  There are two method to solve this problem if you forgot this rule.  One is to remove this file from the local library and the latest remote version will be synchronized.  Removing this pattern from ignore.txt file is another choice.