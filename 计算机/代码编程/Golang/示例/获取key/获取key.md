# 获取key

当有结构体的key不固定式，可使用如下方法解析并获取key

```go
package main

import (
    "encoding/json"
    "fmt"
    "reflect"
)

type Data struct {
    Blocks map[string]interface{} `json:"blocks"`
}

func main() {
    data := `{
        "blocks": {
            "field1": "value1",
            "field2": "value2",
            "field3": 123
        }
    }`

    var d Data
    if err := json.Unmarshal([]byte(data), &d); err != nil {
        panic(err)
    }

    // 使用反射获取blocks字段中的所有字段名
    v := reflect.ValueOf(d.Blocks)
    for _, key := range v.MapKeys() {
        fmt.Println(key.String())
    }
}

```
