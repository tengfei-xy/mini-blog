# POST提交

## 目录

-   [POST 提交](#POST-提交)
    -   [使用http.Post方式（application/x-www-form-urlencoded）](#使用httpPost方式applicationx-www-form-urlencoded)
    -   [http.PostForm方式](#httpPostForm方式)

# POST 提交

## 使用http.Post方式（application/x-www-form-urlencoded）

```go

func httpPost() {
    resp, err := http.Post("http://www.01happy.com/demo/accept.php",
        "application/x-www-form-urlencoded",
        strings.NewReader("name=cjb"))
    if err != nil {
        fmt.Println(err)
    }
 
    defer resp.Body.Close()
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        // handle error
    }
 
    fmt.Println(string(body))
}
```

## http.PostForm方式

```go
func httpPostForm() {
    resp, err := http.PostForm("http://www.01happy.com/demo/accept.php",
        url.Values{"key": {"Value"}, "id": {"123"}})
 
    if err != nil {
        // handle error
    }
 
    defer resp.Body.Close()
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        // handle error
    }
 
    fmt.Println(string(body))
 
}
```
