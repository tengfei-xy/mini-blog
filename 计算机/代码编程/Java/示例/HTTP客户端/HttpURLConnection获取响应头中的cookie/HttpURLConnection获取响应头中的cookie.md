# HttpURLConnection获取响应头中的cookie

仅仅输出cookie

```java
        Map<String, List<String>> headers = conn.getHeaderFields();
        StringBuilder cookies = new StringBuilder();
            List<String> list = headers.get("Set-Cookie");
            for (String line : list){
                cookies.append(line, 0, line.indexOf(";")+2);
            }
        System.out.printf(cookies.toString());
```

单个cookie

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.List;
import java.util.Map;

public class CookieExample {
    public static void main(String[] args) {
        try {
            // 创建URL对象
            URL url = new URL("http://example.com");
            
            // 打开连接
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            
            // 发送GET请求
            connection.setRequestMethod("GET");
            
            // 获取响应码
            int responseCode = connection.getResponseCode();
            System.out.println("Response Code: " + responseCode);
            
            // 获取响应头
            Map<String, List<String>> headers = connection.getHeaderFields();
            
            // 输出所有响应头字段
            //for (Map.Entry<String, List<String>> entry : headers.entrySet()) {
            //    String headerName = entry.getKey()
            //    List<String> headerValues = entry.getValue();
            //    System.out.println(headerName + ": " + headerValues);
            //}
            
            // 输出Cookie字段
            List<String> cookies = headers.get("Set-Cookie");
            if (cookies != null) {
                System.out.println("Cookies:");
                for (String cookie : cookies) {
                    System.out.println(cookie);
                }
            }
            
            // 读取响应内容
            BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String line;
            StringBuilder response = new StringBuilder();
            while ((line = reader.readLine()) != null) {
                response.append(line);
            }
            reader.close();
            
            // 输出响应内容
            System.out.println("Response: " + response.toString());
            
            // 断开连接
            connection.disconnect();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

多个cookie

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.List;
import java.util.Map;

public class MultipleCookieExample {
    public static void main(String[] args) {
        try {
            // 创建URL对象
            URL url = new URL("http://example.com");
            
            // 打开连接
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            
            // 发送GET请求
            connection.setRequestMethod("GET");
            
            // 获取响应码
            int responseCode = connection.getResponseCode();
            System.out.println("Response Code: " + responseCode);
            
            // 获取响应头
            Map<String, List<String>> headers = connection.getHeaderFields();
            
            // 输出所有响应头字段
            //for (Map.Entry<String, List<String>> entry : headers.entrySet()) {
            //    String headerName = entry.getKey();
            //    List<String> headerValues = entry.getValue();
            //    System.out.println(headerName + ": " + headerValues);
            //}
            
            // 输出Cookie字段
            List<String> cookieHeaders = headers.get("Set-Cookie");
            if (cookieHeaders != null) {
                System.out.println("Cookies:");
                for (String cookieHeader : cookieHeaders) {
                    String[] cookies = cookieHeader.split(";"); // 使用分号进行拆分
                    for (String cookie : cookies) {
                        System.out.println(cookie.trim()); // 去除前后空格
                    }
                }
            }
            
            // 读取响应内容
            BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String line;
            StringBuilder response = new StringBuilder();
            while ((line = reader.readLine()) != null) {
                response.append(line);
            }
            reader.close();
            
            // 输出响应内容
            System.out.println("Response: " + response.toString());
            
            // 断开连接
            connection.disconnect();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.List;
import java.util.Map;

public class CookieExample {

public static void main(String\[] args) {

try {

// 创建URL对象

URL url = new URL("[http://example.com](http://example.com "http://example.com")");

// 打开连接

HttpURLConnection connection = (HttpURLConnection) url.openConnection();

// 发送GET请求

connection.setRequestMethod("GET");

// 获取响应码

int responseCode = connection.getResponseCode();

System.out.println("Response Code: " + responseCode);

// 获取响应头

Map\<String, List\<String>> headers = connection.getHeaderFields();

// 输出所有响应头字段

for (Map.Entry\<String, List\<String>> entry : headers.entrySet()) {

String headerName = entry.getKey();

List\<String> headerValues = entry.getValue();

System.out.println(headerName + ": " + headerValues);

}

// 输出Cookie字段

List\<String> cookies = headers.get("Set-Cookie");

if (cookies != null) {

System.out.println("Cookies:");

for (String cookie : cookies) {

System.out.println(cookie);

}

}

// 读取响应内容

BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));

String line;

StringBuilder response = new StringBuilder();

while ((line = reader.readLine()) != null) {

response.append(line);

}

reader.close();

// 输出响应内容

System.out.println("Response: " + response.toString());

// 断开连接

connection.disconnect();

} catch (IOException e) {

e.printStackTrace();

}

}

}
