# 解析JSON

maven

```xml
<dependencies>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.9</version>
        </dependency>
</dependencies>
```

解析

```java
Gson gson = new Gson();
PlayUrlData data = gson.fromJson(response.toString(), PlayUrlData.class);

```
