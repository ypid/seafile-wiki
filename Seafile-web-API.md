Contents

* [Quick Start](#quick-start)
* [Authentication](#authentication)
* [Account Info](#account-info)
* [List Libraries](#list-libraries)
* [View library info](#library-info)
* [Decrypt library](#decrypt-library)
* [Fetch library download info](#download-library)
* [Upload file](#upload-file)
* [Update file](#update-file)
* [List directory entries](#list-directory)
* [New directory](#new-directory)
* [Delete directory](#delete-directory)
* [Get file](#get-file)
* [Delete file](#delete-file)
* [Starred files](#starredfile)
* [Shared Links](#sharedlink)
* [Shared Libraries](#sharedlibs)

# Seafile Web API v2 #

## <a id="quick-start"></a>Quick Start ##

**ping**

    curl https://cloud.seafile.com/api2/ping/

    "pong"

**obtain auth token**

    curl -d "username=username@example.com&password=123456" https://cloud.seafile.com/api2/auth-token/

    {"token": "24fd3c026886e3121b2ca630805ed425c272cb96"}

**auth ping**

    curl -H 'Authorization: Token 24fd3c026886e3121b2ca630805ed425c272cb96' https://cloud.seafile.com/api2/auth/ping/

    "pong"

## <a id="authentication"></a>Authentication ##

**POST** https://cloud.seafile.com/api2/auth-token/

**Request parameters**

* username - The username used in Seafile web loggin.

* password - The password used in Seafile web loggin.

**Sample request**

    curl -d "username=username@example.com&password=123456" https://cloud.seafile.com/api2/auth-token/

**Sample response**

    {"token": "24fd3c026886e3121b2ca630805ed425c272cb96"}
    
**Errors**

* 400 Invalid username or password

## <a id="account-info"></a>Account Info ##

**GET** https://cloud.seafile.com/api2/account/info/

**Request parameters**

* token

**Sample request**

    curl -H "Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd" -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/account/info/

**Sample response**

    {
    "usage": 26038531,
    "total": 104857600,
    "email": "user@example.com"
    }

**Errors**

* 403 Invalid token

## <a id="list-libraries"></a>List Libraries ##

**GET** https://cloud.seafile.com/api2/repos/

**Request parameters**

* token

**Sample request**

    curl -H 'Authorization: Token 24fd3c026886e3121b2ca630805ed425c272cb96' -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/repos/

**Sample response**

    [
    {
        "password_need": false,
        "encrypted": false,
        "name": "fdas",
        "mtime": 1354690832,
        "owner": "username@example.com",
        "root": "af739e46538c06baf99ed46ee07db05ae4e30c8a",
        "size": 31072,
        "type": "repo",
        "id": "e817caa9-7d61-4921-ac2a-0c387cf8576a",
        "desc": "fd"
    },
    {
        "password_need": false,
        "encrypted": false,
        "name": "test1",
        "mtime": 1349759317,
        "owner": "username@example.com",
        "root": "0000000000000000000000000000000000000000",
        "size": 0,
        "type": "repo",
        "id": "1d672103-832d-4565-85c1-2a45d679b74f",
        "desc": "test1"
    }
    ]

## <a id="library-info"></a>View library info ##

**GET** https://cloud.seafile.com/api2/repos/{repo-id}/

**Request parameters**

* token

* repo-id

**Sample request**

    curl -G -H 'Authorization: Token 24fd3c026886e3121b2ca630805ed425c272cb96' -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/repos/632ab8a8-ecf9-4435-93bf-f495d5bfe975/

**Sample response**

    {
    "encrypted": false, 
    "password_need": null, 
    "mtime": null, 
    "owner": "self", 
    "id": "632ab8a8-ecf9-4435-93bf-f495d5bfe975", 
    "size": 1356155, 
    "name": "org", 
    "root": "b5227040de360dd22c5717f9563628fe5510cbce", 
    "desc": "org file", 
    "type": "repo"
    }

## <a id="decrypt-library"></a>Decrypt library ##

**POST** https://cloud.seafile.com/api2/repos/{repo-id}/

**Request parameters**

* token

* password 

**Sample request**

    curl -v -d "password=123" -H 'Authorization: Token e6a33d61954f219a96b60f635cf02717964e4385' -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/repos/0c2465a5-4753-4660-8a22-65abec9ec8d0/
    
**Sample response**

"success"

**Errors**

* 400 Incorrect password

* 409 Repo is not encrypt

* 500 Internal server error

## <a id="download-library"></a>Fetch library download info ##

**GET** https://cloud.seafile.com/api2/repos/{repo-id}/download-info/

**Request parameters**

* token

* repo-id

**Sample request**

    curl -H 'Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d9b477fd' -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/repos/dae8cecc-2359-4d33-aa42-01b7846c4b32/download-info/

**Sample response**

    {
    "applet_root": "https://localhost:13420",
    "relay_addr": "localhost",
    "token": "46acc4d9ca3d6a5c7102ef379f82ecc1edc629e1",
    "repo_id": "dae8cecc-2359-4d33-aa42-01b7846c4b32",
    "relay_port": "10002",
    "encrypted": "",
    "repo_name": "test",
    "relay_id": "8e4b13b49ca79f35732d9f44a0804940d985627c",
    "email": "user@example.com"
    }

## <a id="upload-file"></a>Upload file ##

### Get Upload Link

**GET** https://cloud.seafile.com/api2/repos/{repo-id}/upload-link/

**Request parameters**

* token

* repo-id

**Errors**

    500 Run out of quota

**Sample request**

    curl -H "Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd" https://cloud.seafile.com/api2/repos/99b758e6-91ab-4265-b705-925367374cf0/upload-link/

**Sample response**

    "http://cloud.seafile.com:8082/upload-api/ef881b22"

### Upload File

After getting the upload link, POST to this link for uploading files.

**POST** http://cloud.seafile.com:8082/upload-api/ef881b22

**Errors**

    400 Bad request
    440 Invalid filename
    441 File already exists
    442 File size is too large
    443 Out of quota
    500 Internal server error

**Note**

For python client uploading, see <https://cloud.seafile.com/f/1b0ade6edc/>

## <a id="update-file"></a>Update file ##

### Get Update Link

**GET** https://cloud.seafile.com/api2/repos/{repo-id}/update-link/

**Request parameters**

* token

* repo-id

**Errors**

    500 Run out of quota

**Sample request**

    curl -H "Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd" https://cloud.seafile.com/api2/repos/99b758e6-91ab-4265-b705-925367374cf0/update-link/

**Sample response**

    "http://cloud.seafile.com:8082/update-api/ef881b22"

### Update File

After getting the upload link, POST to this link for uploading files.

**POST** http://cloud.seafile.com:8082/upload-api/ef881b22

**Returns**

The id of the updated file

**Sample response**

    "adc83b19e793491b1c6ea0fd8b46cd9f32e592fc"

**Errors**

    400 Bad request
    440 Invalid filename
    442 File size is too large
    443 Out of quota
    500 Internal server error




### <a id="list-directory"></a>List directory entries ###

**GET** https://cloud.seafile.com/api2/repos/{repo-id}/dir/

* token

* repo-id

* p (optional) The path to a directory. If `p` is missing, then defaults to '/' which is the top directory.

* oid (optional)

**Sample request**

    curl -H "Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d9b477fd" -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/repos/99b758e6-91ab-4265-b705-925367374cf0/dir/?p=/foo

**Sample response**

   If oid is the latest oid of the directory, returns `"uptodate"` , else returns

    [
    {
        "id": "0000000000000000000000000000000000000000",
        "type": "file",
        "mime": "text/x-c",
        "name": "test1.c",
        "size": 0
    },
    {
        "id": "e4fe14c8cda2206bb9606907cf4fca6b30221cf9",
        "type": "file",
        "mime": "text/x-c",
        "name": "test.c",
        "size": 14
    }
    ]

**Errors**

* 404 The path is not exist.
* 440 Repo is encrypted, and password is not provided.
* 520 Operation failed..

### <a id="new-directory"></a>New directory ###

**POST** https://cloud.seafile.com/api2/repos/{repo-id}/dir/

* token

* repo-id

* p

* operation=mkdir (post)

**Sample request**

    curl -d  "operation=mkdir" -v  -H 'Authorization: Tokacd9c6ccb8133606d94ff8e61d99b477fd' -H 'Accept: application/json; charset=utf-8; indent=4' https://cloud.seafile.com/api2/repos/dae8cecc-2359-4d33-aa42-01b7846c4b32/dir/?p=/foo

**Sample response**
   
    ...
    < HTTP/1.0 201 CREATED
    < Location: https://cloud.seafile.com/api2/repos/dae8cecc-2359-4d33-aa42-01b7846c4b32/dir/?p=/foo
    ...

    "success"

**Success**

   Response code 201(Created) is returned, and Location header provides the url of created directory.

**Errors**

* 400 Path is missing or invalid(e.g. p=/)
* 520 Operation failed.

**Notes**

   Newly created directory will be renamed if the name is duplicated.

### <a id="delete-directory"></a>Delete directory ###

**DELETE** https://cloud.seafile.com/api2/repos/{repo-id}/dir/

* token

* repo-id

* p

**Sample request**

    curl -X DELETE -v  -H 'Authorization: Token f2210dacd3606d94ff8e61d99b477fd' -H 'Accept: application/json; charset=utf-8; indent=4' https://cloud.seafile.com/api2/repos/dae8cecc-2359-4d33-aa42-01b7846c4b32/dir/?p=/foo

**Sample response**

    ...
    < HTTP/1.0 200 OK
    ...
    "success"

**Success**

   Response code is 200(OK), and a string `"success"` is returned.
   
**Errors**

* 400 Path is missing or invalid(e.g. p=/)
* 520 Operation failed.

**Note**

   This can also be used to delete file.
   
### <a id="get-file"></a>Get file  ###

**GET** https://cloud.seafile.com/api2/repos/{repo-id}/file/?p=/foo

* token

* repo-id

* p

**Sample request**

    curl  -v  -H 'Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd' -H 'Accept: application/json; charset=utf-8; indent=4' https://cloud.seafile.com/api2/repos/dae8cecc-2359-4d33-aa42-01b7846c4b32/file/?p=/foo.c

**Sample response**

    "https://cloud.seafile.com:8082/files/adee6094/foo.c"

**Errors**

* 400 Path is missing
* 404 File not found
* 520 Operation failed.

### <a id="delete-file"></a>Delete file ###

**DELETE** https://cloud.seafile.com/api2/repos/{repo-id}/file/?p=/foo

* token

* repo-id

* p

**Sample request**

    curl -X DELETE -v  -H 'Authorization: Token f2210dacd3606d94ff8e61d99b477fd' -H 'Accept: application/json; charset=utf-8; indent=4' https://cloud.seafile.com/api2/repos/dae8cecc-2359-4d33-aa42-01b7846c4b32/file/?p=/foo.c

**Sample response**

    ...
    < HTTP/1.0 200 OK
    ...
    "success"

**Errors**

* 400 Path is missing
* 520 Operation failed.

**Note**

   This can also be used to delete directory.

### Batch delete ###

Pipelining over HTTP/1.1 can be used to delete multiple files and directories without losing performance.

A sample request looks like `curl -X DELETE https://cloud.seafile.com/api2/repos/{repo-id}/dir/?p=/foo http://cloud.seafile.com/api2/repos/{repo-id}/dir/?p=/bar`. This code snippet shows how to use Python client to batch delete multiple files and directories. See <http://cloud.seafile.com/f/f7fd5d5b9d/>

## <a id="starredfile"></a>Starred files ##

### List starred files

**GET** https://cloud.seafile.com/api2/starredfiles/

**Request parameters**

* token

**Sample request**

    curl -H "Authorization: Token f2210dacd9c6ccb8133606d94ff8e6199b477fd" -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/starredfiles/

**Sample response**

    [
    {
        "repo": "99b758e6-91ab-4265-b705-925367374cf0",
        "mtime": 1355198150,
        "org": -1,
        "path": "/foo/bar.doc",
        "dir": false,
        "size": 0
    },
    {
        "repo": "99b758e6-91ab-4265-b705-925367374cf0",
        "mtime": 1353751237,
        "org": -1,
        "path": "/add_folder-blue.png",
        "dir": false,
        "size": 3170
    }
    ]

### Star a file

**POST** https://cloud.seafile.com/api2/starredfiles/

**Request parameters**

* token

* repo_id (post)

* p (post)

**Sample request**

    curl -v -d "repo_id=dae8cecc-2359-4d33-aa42-01b7846c4b32&p=/foo.md" -H 'Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd' -H 'Accept: application/json; charset=utf-8; indent=4' https://cloud.seafile.com/api2/starredfiles/

**Sample response**

    ...
    < HTTP/1.0 201 CREATED
    < Location: https://cloud.seafile.com/api2/starredfiles/
    ...
    "success"

**Success**

   Response code is 201(Created) and Location header provides url of starred file list.
    
**Errors**

* 400 `repo_id` or `p` is missing, or `p` is not valid file path(e.g. /foo/bar/).
       
### Unstar a file

**DELETE** https://cloud.seafile.com/api2/starredfiles/

**Request parameters**

* token

* repo_id

* p

**Sample request**

    curl -X DELETE -v  -H 'Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd' -H 'Accept: application/json; charset=utf-8; indent=4' 'https://cloud.seafile.com/api2/starredfiles/?repo_id=dae8cecc-2359-4d33-aa42-01b7846c4b32&p=/foo.md'

**Sample response**

    ...
    < HTTP/1.0 200 OK
    ...
    "success"

**Success**

   Response code is 200(OK), and a string named "success" is returned.
   
**Errors**

* 400 `repo_id` or `p` is missing, or `p` is not valid file path(e.g. /foo/bar/).

## <a id="sharedlink"></a>File shared link ##

### Obtain file shared link ###

**PUT** https://cloud.seafile.com/api2/repos/{repo-id}/file/shared-link/

**Request parameters**

* token

* repo-id

* p Path to the file

**Sample request**

    curl -v  -X PUT -d "p=/foo.md" -H 'Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd' -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/repos/afc3b694-7d4c-4b8a-86a4-89c9f3261b12/file/shared-link/

**Sample response**

    ...
    < HTTP/1.0 201 CREATED
    < Location: https://cloud.seafile.com/f/9b437a7e55/
    ...

**Success**

    Response code 201(Created) is returned and the Location header provides shared link.

**Errors**

* 400 Path is missing
* 500 Internal server error

## <a id="sharedlibs"></a>Shared Libraries ##

### List shared libraries ###

**GET** https://cloud.seafile.com/api2/shared-repos/

**Request parameters**

* token

**Sample request**

    curl -v -H 'Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd' -H 'Accept: application/json; indent=4' https://cloud.seafile.com/api2/shared-repos/
    
**Sample response**

    [{"repo_id": "7d42522b-1f6f-465d-b9c9-879f8eed7c6c", "share_type": "personal", "permission": "rw", "encrypted": false, "user": "user@example.com", "last_modified": 1361072500, "repo_desc": "ff", "group_id": 0, "repo_name": "\u6d4b\u8bd5\u4e2d\u6587pdf"}, {"repo_id": "79bb29cd-b683-4844-abaf-433952723ca5", "share_type": "group", "permission": "rw", "encrypted": false, "user": "user@example.com", "last_modified": 1359182468, "repo_desc": "test", "group_id": 1, "repo_name": "test_enc"}]

### Unshare a library ###

**DELETE** https://cloud.seafile.com/api2/share-repos/{repo-id}/

**Request parameters**

* token

* share_type

* user

* group_id

**Sample request**

    curl -X DELETE -H 'Authorization: Token f2210dacd9c6ccb8133606d94ff8e61d99b477fd' "https://cloud.seafile.com/api2/shared-repos/7d42522b-1f6f-465d-b9c9-879f8eed7c6c/?share_type=personal&user=user@example.com&group_id=0"

**Sample response**

    "success"
