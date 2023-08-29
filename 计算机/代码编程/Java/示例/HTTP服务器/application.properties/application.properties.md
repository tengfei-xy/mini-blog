# application.properties

```ini
# 监听端口
server.port=16825
# 静态资源的默认位置,resources/ 下
spring.web.resources.static-locations=classpath:/web/
# 日志等级
logging.level.root=DEBUG
# 日志格式
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{36}.%M:%L - %msg%n
# 启动颜色输出
spring.output.ansi.enabled=ALWAYS

```
