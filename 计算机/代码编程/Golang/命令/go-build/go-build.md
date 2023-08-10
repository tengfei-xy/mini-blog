# go-build

## 目录

-   [ldflags参数](#ldflags参数)

shell下

```go
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build main.go
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go
CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build main.go
```

cmd下

```纯文本
SET CGO_ENABLED=0
SET GOOS=darwin
SET GOARCH=amd64
go build main.go
```

# ldflags参数

-s -w -X main.KEYimg=\${key} -H windowsgui

| 选项 | 说明                       |
| -- | ------------------------ |
| -s | disable symbol table     |
| -w | disable DWARF generation |
| -X | 根据包名赋值变量，需要在go中声明变量      |
| -H | windows系统下使用gui，默认为cui   |
