# 解析html

## 目录

-   [Jsoup](#Jsoup)
    -   [方法示例](#方法示例)

# Jsoup

pom.xml

```xml
<dependencies>
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.15.3</version>
</dependency>
</dependencies>

```

示例代码

```java
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

public class HtmlParser {
    public static void main(String[] args) throws Exception {
        String html = "<html><head><title>Jsoup Example</title></head>"
                + "<body><p>Jsoup is a Java HTML parser</p></body></html>";
        Document doc = Jsoup.parse(html);
        Element title = doc.select("title").first();
        System.out.println("Title: " + title.text());

        Elements paragraphs = doc.select("p");
        for (Element paragraph : paragraphs) {
            System.out.println("Paragraph: " + paragraph.text());
        }
    }
}
```

## 方法示例

获取id=bilibiliPlayer的元素

```java
Element div = doc.select("#bilibiliPlayer").first();
```

获取span class="A B"的元素

```java
Element div = doc.select("span.A.B").first();
```

输出元素数

```java
Elements paragraphs = container.getElementsByTag("p");
int paragraphCount = paragraphs.size();
```

同时选择多个类

```java
Elements elements = doc.select(".A, .B");
```
