# JSON

[json转实体类](http://www.esjson.com/jsontopojo.html "json转实体类")

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

解析到class

```java
Gson gson = new Gson();
PlayUrlData data = gson.fromJson(response.toString(), PlayUrlData.class);

```

直接提取字段

```java
import com.google.gson.*;

public class GsonExtractFieldsExample {
    public static void main(String[] args) {
        String json = "{\"name\":\"John\",\"age\":30,\"city\":\"New York\"}";

        JsonParser parser = new JsonParser();
        JsonElement element = parser.parse(json);

        if (element.isJsonObject()) {
            JsonObject jsonObject = element.getAsJsonObject();

            JsonElement nameElement = jsonObject.get("name");
            if (nameElement != null && nameElement.isJsonPrimitive()) {
                String name = nameElement.getAsString();
                System.out.println("Name: " + name);
            }

            JsonElement ageElement = jsonObject.get("age");
            if (ageElement != null && ageElement.isJsonPrimitive()) {
                int age = ageElement.getAsInt();
                System.out.println("Age: " + age);
            }
        }
    }
}
```
