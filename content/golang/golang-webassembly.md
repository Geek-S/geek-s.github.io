---
title: "Golang webassembly"
date: 2022-01-21T12:40:28+08:00
draft: false
tags: ["golang", "wasm"]
categories: ["Golang"]

showShare: false
---

# Quick start

Go的版本需要 `1.11` 及以上版本

```
package main

import "fmt"

func main() {
	fmt.Println("Hello, WebAssembly!")
}
```

设置`GOOS=js` 、 `GOARCH=wasm`环境变量后编译

```
$ GOOS=js GOARCH=wasm go build -o main.wasm
```

在浏览器执行main.wasm，需要JS文件：

```
$ cp "$(go env GOROOT)/misc/wasm/wasm_exec.js" .
```

创建index.html文件：

```
<html>
  <head>
    <meta charset="utf-8"/>
    <script src="wasm_exec.js"></script>
    <script>
      const go = new Go();
        WebAssembly.instantiateStreaming(fetch("main.wasm"), go.importObject).then((result) => {
            go.run(result.instance);
        });
    </script>
  </head>
  <body></body>
</html>
```

如果浏览器不支持WebAssembly.instantiateStreaming，可以使用[polyfill](https://github.com/golang/go/blob/b2fcfc1a50fbd46556f7075f7f1fbf600b5c9e5d/misc/wasm/wasm_exec.html#L17-L22)

将`index.html`、`wasm_exec.js`、`main.wasm`放在web服务器（[goexec](https://github.com/shurcooL/goexec#goexec)）

```
# install goexec: go get -u github.com/shurcooL/goexec
$ goexec 'http.ListenAndServe(`:8080`, http.FileServer(http.Dir(`.`)))'
```

![](img/golang-wasm.jpg)

# 参考

- [WebAssembly@github](https://github.com/golang/go/wiki/WebAssembly)
