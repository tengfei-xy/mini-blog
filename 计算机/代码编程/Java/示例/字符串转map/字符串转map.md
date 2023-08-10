# 字符串转map

```java
import java.util.HashMap;
import java.util.Map;

public class StringToMapExample {
    public static void main(String[] args) {
        String input = "ret=0&err=auth-failure&freehosinfo=未授权的IP60.12.216.130";

        // 创建一个HashMap对象来存储键值对
        Map<String, String> map = new HashMap<>();

        // 按照"&"符号拆分字符串
        String[] keyValuePairs = input.split("&");

        // 遍历键值对数组
        for (String pair : keyValuePairs) {
            // 按照"="符号拆分键值对
            String[] entry = pair.split("=");

            // 将键值对添加到Map中
            if (entry.length == 2) {
                String key = entry[0];
                String value = entry[1];
                map.put(key, value);
            }
        }

        // 打印Map中的键值对
        for (Map.Entry<String, String> entry : map.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
    }
}

```
