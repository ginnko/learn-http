### http学习

#### httpie

1. `--print HB`：打印请求头和请求体
2. `http -v -a ginnko -j post https://api.github.com/repos/ginnko/learn-http/issues title='test' body='this is a test.'`

#### http

1. 如果请求头中包含`Accept:application/json`，那么返回头中将包含`Content-type:application/json`
2. 比如向这个api发出请求`https://api.github.com/gitignore/templates/Node`，如果附带`Accept：text/html`,将返回`415 Unsupported Media Type`
3. 400 bad request
4. head方法和get方法基本一样，除了不会返回body
5. 201 created 返回头中有一个location，用来表示新建的资源的地址，比如：`Location: https://api.github.com/repos/ginnko/learn-http/issues/4`
6. 创建一个fork： `http -v -a ginnko post https://api.github.com/repos/facebook/react/forks`

    返回：`204 Accepted`同时没有location这个返回头，这个状态表明请求已经被接受但是资源还没有立刻创建完成。

7. 204 No Content

  查看一个gist是否有点赞：`http -a petejohanson -h get https://api.github.com/gists/14b45f67868cc78ea9085acd06181196/star`，如果返回这个状态就表示有星星。404 Not Found表示没有。

8. 206 Partial Content

  `http -h get https://avatars.githubusercontent.com/u/25996192?v3 Range:bytes='0-100'`。这里使用了一个`Range`头，获取一个比特的范围。返回头中会有一个`Content-Range: bytes 0-100/29554`Content-Range表示当前获取得范围以及总大小。这个头可以用来表示当前资源的大小以及剩余资源的大小。
