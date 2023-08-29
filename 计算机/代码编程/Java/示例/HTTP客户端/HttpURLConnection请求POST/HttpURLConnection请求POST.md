# HttpURLConnection请求POST

注意HttpURLConnection无法用于http2,且用于jdk8及以下

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.HashMap;
import java.util.Map;
import java.nio.charset.StandardCharsets;

public class HttpPostRequest {
    public static void main(String[] args) throws Exception {
        // 设置请求的URL地址
        URL url = new URL("http://example.com/post");
        
        // 创建连接对象
        HttpURLConnection con = (HttpURLConnection) url.openConnection();
        
        // 设置请求方法为POST
        con.setRequestMethod("POST");
        
        // 设置请求头
        con.setRequestProperty("Content-Type", "application/json");
        
        // 设置请求体
        Map<String, String> requestBody = new HashMap<>();
        requestBody.put("key1", "value1");
        requestBody.put("key2", "value2");
        String requestBodyString = new Gson().toJson(requestBody);
        con.setDoOutput(true);
        con.getOutputStream().write(requestBodyString.getBytes(StandardCharsets.UTF_8));
        
        // 发送请求并获取响应
        int status = con.getResponseCode();
        BufferedReader in = new BufferedReader(
                new InputStreamReader(con.getInputStream()));
        String inputLine;
        StringBuffer response = new StringBuffer();
        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }
        in.close();
        
        // 打印响应结果
        System.out.println("HTTP状态码：" + status);
        System.out.println("响应内容：" + response.toString());
    }
}
```
