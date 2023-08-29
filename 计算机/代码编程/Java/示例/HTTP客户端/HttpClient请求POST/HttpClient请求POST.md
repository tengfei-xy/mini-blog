# HttpClient请求POST

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.net.http.HttpHeaders;
import java.net.http.HttpMethod;
import java.net.http.HttpRequest.BodyPublishers;
import java.net.http.HttpResponse.BodyHandlers;
import java.util.HashMap;
import java.util.Map;

public class HttpClientExample {

    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();
        sendPostRequest(client);
    }

    private static void sendPostRequest(HttpClient client) {
        try {
            // 请求URL
            URI url = new URI("https://example.com");

            // 请求头
            Map<String, String> headers = new HashMap<>();
            headers.put("Content-Type", "application/json");
            headers.put("Authorization", "Bearer your_access_token");

            // 请求数据
            String requestBody = "{\"name\":\"John\",\"age\":30}";

            // 创建POST请求
            HttpRequest request = HttpRequest.newBuilder()
                    .uri(url)
                    .POST(BodyPublishers.ofString(requestBody))
                    .headers(buildHeaders(headers))
                    .build();

            // 发送请求并获取响应
            HttpResponse<String> response = client.send(request, BodyHandlers.ofString());

            // 输出响应结果
            int statusCode = response.statusCode();
            HttpHeaders responseHeaders = response.headers();
            String responseBody = response.body();

            System.out.println("Status Code: " + statusCode);
            System.out.println("Response Headers: " + responseHeaders);
            System.out.println("Response Body: " + responseBody);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static HttpHeaders buildHeaders(Map<String, String> headers) {
        HttpHeaders.Builder builder = HttpHeaders.newBuilder();

        for (Map.Entry<String, String> entry : headers.entrySet()) {
            builder.header(entry.getKey(), entry.getValue());
        }

        return builder.build();
    }
}
```
