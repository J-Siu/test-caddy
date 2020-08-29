<!-- TOC -->

- [Caddy](#caddy)
  - [CaddyCommand](#caddycommand)
- [Curl](#curl)
  - [Result](#result)
  - [Curl Command](#curl-command)
  - [Curl Output](#curl-output)

<!-- /TOC -->

### Caddy

https://github.com/caddyserver/caddy/actions/runs/223247096

#### CaddyCommand

Caddy:

```sh
$ ./caddy version
(devel)
$ ./caddy run -config Caddyfile-M5
```

### Curl

#### Result

Test#|Request|Expected|Actual
---|---|---|---
M5T1 | http://localhost:1314/dir | no redir (not exit in /blog) | no redir
M5T2 | http://localhost:1314/dir2dir | no redir (exist at /) | no redir
M5T3 | http://localhost:1314/dir2file | no redir (exist at /) | no redir
M5T4 | http://localhost:1314/file | no redir (not exit in /blog) | no redir
M5T5 | http://localhost:1314/file2dir | no redir (exist at /) | no redir
M5T6 | http://localhost:1314/file2file | no redir (exist at /) | no redir
M5T7 | http://localhost:1314/blogdir | redir /blog/blogdir/ | redir /blog/blogdir/
M5T8 | http://localhost:1314/blogfile | redir /blog/blogfile | redir /blog/blogfile

#### Curl Command

```sh
./curl.sh 5
```

#### Curl Output

Output:

```sh
===
M5 Testing without trailing /
===
M5T1 REQ:http://localhost:1314/dir expected redir:no
> GET /dir HTTP/1.1
< HTTP/1.1 308 Permanent Redirect
< Location: /dir/
> GET /dir/ HTTP/1.1
< HTTP/1.1 200 OK
This is /dir/index.html* Closing connection 0
---
M5T2 REQ:http://localhost:1314/dir2dir expected redir:no
> GET /dir2dir HTTP/1.1
< HTTP/1.1 308 Permanent Redirect
< Location: /dir2dir/
> GET /dir2dir/ HTTP/1.1
< HTTP/1.1 200 OK
This is /dir2dir/index.html* Closing connection 0
---
M5T3 REQ:http://localhost:1314/dir2file expected redir:no
> GET /dir2file HTTP/1.1
< HTTP/1.1 308 Permanent Redirect
< Location: /dir2file/
> GET /dir2file/ HTTP/1.1
< HTTP/1.1 200 OK
This is /dir2file/index.html* Closing connection 0
---
M5T4 REQ:http://localhost:1314/file expected redir:no
> GET /file HTTP/1.1
< HTTP/1.1 200 OK
This is /file* Closing connection 0
---
M5T5 REQ:http://localhost:1314/file2dir expected redir:no
> GET /file2dir HTTP/1.1
< HTTP/1.1 200 OK
This is /file2dir* Closing connection 0
---
M5T6 REQ:http://localhost:1314/file2file expected redir:no
> GET /file2file HTTP/1.1
< HTTP/1.1 200 OK
This is /file2file* Closing connection 0
---
M5T7 REQ:http://localhost:1314/blogdir expected redir:yes
> GET /blogdir HTTP/1.1
< HTTP/1.1 302 Found
< Location: /blog/blogdir
> GET /blog/blogdir HTTP/1.1
< HTTP/1.1 308 Permanent Redirect
< Location: /blog/blogdir/
> GET /blog/blogdir/ HTTP/1.1
< HTTP/1.1 200 OK
This is /blog/blogdir/index.html* Closing connection 0
---
M5T8 REQ:http://localhost:1314/blogfile expected redir:yes
> GET /blogfile HTTP/1.1
< HTTP/1.1 302 Found
< Location: /blog/blogfile
> GET /blog/blogfile HTTP/1.1
< HTTP/1.1 200 OK
This is /blog/blogfile* Closing connection 0
---
```