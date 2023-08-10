# 计算MD5

```java
import java.security.MessageDigest;

public class MD5Example {
    public static void main(String[] args) {
        String input = "Hello, World!";

        try {
            // 创建MessageDigest对象并指定算法为MD5
            MessageDigest md = MessageDigest.getInstance("MD5");
            
            // 将输入字符串转换为字节数组
            byte[] inputBytes = input.getBytes();
            
            // 计算MD5哈希值
            byte[] hashBytes = md.digest(inputBytes);
            
            // 将字节数组转换为十六进制字符串
            StringBuilder hexString = new StringBuilder();
            for (byte b : hashBytes) {
                String hex = Integer.toHexString(0xFF & b);
                if (hex.length() == 1) {
                    hexString.append('0');
                }
                hexString.append(hex);
            }
            
            String md5Hash = hexString.toString();
            
            System.out.println("MD5哈希值：" + md5Hash);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
