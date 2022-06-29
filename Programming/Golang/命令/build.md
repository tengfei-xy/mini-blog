# ldflags

-s -w -X main.KEYimg=${key} -H windowsgui

| 选项 | 说明                            |
| ---- | ------------------------------- |
| -s   | disable symbol table            |
| -w   | disable DWARF generation        |
| -X   | 根据包名赋值变量                |
| -H   | windows系统下使用gui，默认为cui |

