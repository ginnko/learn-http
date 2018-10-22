### http学习

#### httpie

1. `--print HB`：打印请求头和请求体
2. `http -v -a ginnko -j post https://api.github.com/repos/ginnko/learn-http/issues title='test' body='this is a test.'`

#### http

1. 如果请求头中包含`Accept:application/json`，那么返回头中将包含`Content-type:application/json`
2. 比如向这个api发出请求`https://api.github.com/gitignore/templates/Node`，如果附带`Accept：text/html`,将返回`415 Unsupported Media Type`
3. 400 bad request
4. head方法和get方法基本一样，除了不会返回body
