# 格式化日志-logrus

```go
package main

import (
  "github.com/sirupsen/logrus"
)

func main() {
  logrus.SetReportCaller(true)
  logrus.SetFormatter(&logrus.TextFormatter{FullTimestamp: true, TimestampFormat: "2006-01-02 15:04:05"})
  logrus.Infof("running!")
}

```

对于winows的换行符

```go
// 设置
  log = logrus.New()
  log.SetFormatter(&logrus.TextFormatter{
    TimestampFormat: "2006-01-02 15:03:04",
    DisableQuote:    true,
    CallerPrettyfier: func(frame *runtime.Frame) (function string, file string) {
      return "", fmt.Sprintf("%s:%d", filepath.Base(frame.File), frame.Line)
    },
  })

// 输出测试
log.Info("233\r")
```

同时输出到日志和控制台

```go
// 设置
log = logrus.New()
file, _ := os.OpenFile(file_log, os.O_RDWR|os.O_CREATE|os.O_APPEND, os.ModePerm)
mw := io.MultiWriter(os.Stdout, file)
log.SetOutput(mw)

// 输出测试
log.Info("233")
```
