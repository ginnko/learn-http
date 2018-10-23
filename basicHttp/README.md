**这个文件夹的记录使用`httpie`完成完成相关操作，主要记录了常见的状态码及对应的情景以及自己没太使用过的请求方法。**

#### httpie

1. `--print HB`：打印请求头和请求体
2. `http -v -a ginnko -j post https://api.github.com/repos/ginnko/learn-http/issues title='test' body='this is a test.'`
3. `-F`或者`--follow`自动重定向

#### http

1. 如果请求头中包含`Accept:application/json`，那么返回头中将包含`Content-type:application/json`
2. 比如向这个api发出请求`https://api.github.com/gitignore/templates/Node`，如果附带`Accept：text/html`,将返回`415 Unsupported Media Type`
3. 400 bad request
4. head方法和get方法基本一样，除了不会返回body

#### 200系列

1. 201 created 返回头中有一个location，用来表示新建的资源的地址，比如：`Location: https://api.github.com/repos/ginnko/learn-http/issues/4`
2. 创建一个fork： `http -v -a ginnko post https://api.github.com/repos/facebook/react/forks`

    返回：`202 Accepted`同时没有location这个返回头，这个状态表明请求已经被接受但是资源还没有立刻创建完成。

3. 204 No Content

  查看一个gist是否有点赞：`http -a petejohanson -h get https://api.github.com/gists/14b45f67868cc78ea9085acd06181196/star`，如果返回这个状态就表示有星星。404 Not Found表示没有。

4. 206 Partial Content

  `http -h get https://avatars.githubusercontent.com/u/25996192?v3 Range:bytes='0-100'`。这里使用了一个`Range`头，获取一个比特的范围。返回头中会有一个`Content-Range: bytes 0-100/29554`Content-Range表示当前获取得范围以及总大小。这个头可以用来表示当前资源的大小以及剩余资源的大小。

#### 300系列

1. 301 Moved Permanently：比如`http get https://api.github.com/repos/petejohanson/egghead-api-repo/issues`。会给出一个location头部，指明当前资源正确的地址。使用`get`方法时。
2. 307 Moved Temperarily：使用`POST`时，不会显示301，而是显示307。
3. 304 Not Modified  比如这样请求：`http get https://api.github.com/users/ginnko If-None-Match:'W/"c354578cb79c0199f8bc8224cce66464"'`。使用`If-None-Match`请求头时，浏览器使用自己的缓存中的资源作为请求的响应。

#### 400系列

1. 404 Not Found
2. 400 Bad Request 比如`http -v -a ginnko -f post https://api.github.com/repos/ginnko/learn-http/issues title='test'`使用urlencodeform格式不行，必须使用json格式。
3. 422 Unprocessable Entity  比如`http -v -a ginnko -f post https://api.github.com/repos/ginnko/learn-http/issues`，没有传入正确的内容时。
4. 401 Unauthorized 比如`http -h get https://api.github.com/user`，表示获取当前用户，由于没有验证，所以返回这个状态。
5. 403 Forbidden


#### 500系列

1. 500 Internal Server Error
2. 503 Service Unavailable 比如服务器不可用或者在重启过程中

#### 请求方法

1. delete 方法，删除一次之后可能返回204，再次删除相同的资源同样返回204，表明无论资源是否存在，都可以使用delete方法。
2. patch方法，第一次听说这个方法诶，用于更新存在的资源，比如`http -v -a ginnko patch https://api.github.com/repos/ginnko/learn-http/issues/5 title='patch method test' body='successful?'`。返回 200 OK。
3. options方法，返回我们发送请求资源时，可以使用的头部以及方法。