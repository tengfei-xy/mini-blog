# HttpsURLConnection请求HTTPS

> HttpsURLConnection注意
> 1\. 无法用于http2

```java
import javax.net.ssl.HttpsURLConnection;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.URL;

public class HttpsExample {

    public static void main(String[] args) {
        String url = "https://example.com";

        try {
            // 创建URL对象
            URL apiUrl = new URL(url);

            // 打开HTTPS连接
            HttpsURLConnection connection = (HttpsURLConnection) apiUrl.openConnection();

            // 可选：设置请求方法，默认为GET
            connection.setRequestMethod("GET");

            // 获取响应代码
            int responseCode = connection.getResponseCode();
            System.out.println("Response Code: " + responseCode);

            // 读取响应内容
            InputStream inputStream = connection.getInputStream();
            BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
            String line;
            StringBuilder response = new StringBuilder();
            while ((line = reader.readLine()) != null) {
                response.append(line);
            }
            reader.close();

            // 输出响应内容
            System.out.println("Response Body: " + response.toString());

            // 关闭连接
            connection.disconnect();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
