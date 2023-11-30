# 简易HTTP

```go
package main

import (
  "fmt"
  "net/http"
)

func main() {
  // 设置路由规则和处理函数
  http.HandleFunc("/", handleRequest)

  // 启动服务器并监听端口
  err := http.ListenAndServe(":8080", nil)
  if err != nil {
    fmt.Println("服务器启动失败：", err)
  }
}

// 处理函数，对所有请求返回 "Hello, World!"
func handleRequest(w http.ResponseWriter, r *http.Request) {
  fmt.Fprint(w, "Hello, World!")
}
```
