# POST提交

## 目录

-   [一般方式](#一般方式)
    -   [使用http.Post方式（application/x-www-form-urlencoded）](#使用httpPost方式applicationx-www-form-urlencoded)
    -   [http.PostForm方式](#httpPostForm方式)

# 一般方式

```go
func getKeyCompanySpider(cookie string, id string) ([]byte, bool) {

  client := &http.Client{}
  req, err := http.NewRequest("GET", fmt.Sprintf("https://logistics.tungee.com/cgi/logistics-common/api/enterprise/info/detail?enterprise_id=%s", id), nil)
  if err != nil {
    log.Error(err)
    return nil, false
  }
  req.Header.Set("Authority", `logistics.tungee.com`)
  req.Header.Set("Accept", `*/*`)
  req.Header.Set("Accept-Language", `zh-CN,zh;q=0.9`)
  req.Header.Set("cache-control", `no-cache`)
  req.Header.Set("Cookie", cookie)
  req.Header.Set("Origin", `https://logistics.tungee.com`)
  req.Header.Set("pragma", `no-cache`)
  req.Header.Set("Referer", fmt.Sprintf("https://logistics.tungee.com/enterprise-details/%s/enterprise-information/basic-information", id))
  req.Header.Set("Sec-Fetch-Dest", `empty`)
  req.Header.Set("Sec-Fetch-Mode", `cors`)
  req.Header.Set("Sec-Fetch-Site", `same-origin`)
  req.Header.Set("User-Agent", `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36`)
  req.Header.Set("sec-ch-ua", `"Not.A/Brand";v="8", "Chromium";v="114", "Google Chrome";v="114"`)
  req.Header.Set("sec-ch-ua-mobile", `?0`)
  req.Header.Set("sec-ch-ua-platform", `"macOS"`)

  resp, err := client.Do(req)
  if err != nil {
    log.Errorf("内部错误:%v", err)
    return nil, false
  }
  defer resp.Body.Close()

  if resp.StatusCode != 200 {
    log.Errorf("状态码:%d cooie:%s", resp.StatusCode, cookie)
    return nil, false
  }

  resp_data, err := io.ReadAll(resp.Body)
  if err != nil {
    log.Errorf("内部错误:%v", err)
    return nil, false
  }
  // log.Info(string(resp_data))
  return resp_data, true
}
```

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
