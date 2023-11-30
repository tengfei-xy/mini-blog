# 解析html

使用模块

```sql
github.com/PuerkitoBio/goquery
```

-   查找示例
    ```sql
    package main

    import (
      "fmt"
      "log"
      "strings"

      "github.com/PuerkitoBio/goquery"
    )

    func main() {
      // 假设你已经获取到了 HTML 文档的内容，并将其保存在变量 html 中

      // 创建一个新的 Document 对象
      doc, err := goquery.NewDocumentFromReader(strings.NewReader(html))
      if err != nil {
        log.Fatal(err)
      }

      // 使用 CSS 选择器查找 class 属性中包含 s-search-results 类的 div 元素
      doc.Find("div[class~=s-search-results]").Each(func(i int, s *goquery.Selection) {
        // 处理找到的 div 元素
        fmt.Println(s.Text())
      })
    }
    ```
-   查找**包含某类的div**
    ```go
     doc.Find("div[class~=s-search-results]").Each(func(i int, s *goquery.Selection) {
        // 处理找到的 div 元素
        fmt.Println(s.Text())
      })
    ```
-   查找**包含data属性**的div
    ```go
    doc.Find("div[data]").Each(func(i int, s *goquery.Selection) {
        // 处理找到的 div 元素
        fmt.Println(s.Text())
      })
    ```
