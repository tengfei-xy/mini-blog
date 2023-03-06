普通占位符

|      |                    |
| ---- | ------------------ |
| %v   | 相应值的默认格式。 |
|      |                    |
|      |                    |



```javascript
占位符                        说明                        举例                                        输出
                                Printf("%v", site)，Printf("%+v", site)    {studygolang}，{Name:studygolang}
        在打印结构体时，“加号”标记（%+v）会添加字段名
%#v        相应值的Go语法表示                            Printf("#v", site)                        main.Website{Name:"studygolang"}
%T        相应值的类型的Go语法表示                        Printf("%T", site)                        main.Website
%%        字面上的百分号，并非值的占位符                    Printf("%%")                            %
```

复制

布尔占位符

```javascript
占位符                        说明                        举例                                        输出
%t        单词 true 或 false。                            Printf("%t", true)                        true
```

复制

整数占位符

```javascript
占位符                        说明                        举例                                    输出
%b        二进制表示                                    Printf("%b", 5)                        101
%c        相应Unicode码点所表示的字符                    Printf("%c", 0x4E2D)                中
%d        十进制表示                                    Printf("%d", 0x12)                    18
%o        八进制表示                                    Printf("%d", 10)                    12
%q        单引号围绕的字符字面值，由Go语法安全地转义        Printf("%q", 0x4E2D)                '中'
%x        十六进制表示，字母形式为小写 a-f                Printf("%x", 13)                    d
%X        十六进制表示，字母形式为大写 A-F                Printf("%x", 13)                    D
%U        Unicode格式：U+1234，等同于 "U+%04X"            Printf("%U", 0x4E2D)                U+4E2D
```

复制

浮点数和复数的组成部分（实部和虚部）

```javascript
占位符                        说明                                                举例                                    输出
%b        无小数部分的，指数为二的幂的科学计数法，与 strconv.FormatFloat    
        的 'b' 转换格式一致。例如 -123456p-78
%e        科学计数法，例如 -1234.456e+78                                    Printf("%e", 10.2)                            1.020000e+01
%E        科学计数法，例如 -1234.456E+78                                    Printf("%e", 10.2)                            1.020000E+01
%f        有小数点而无指数，例如 123.456                                    Printf("%f", 10.2)                            10.200000
%g        根据情况选择 %e 或 %f 以产生更紧凑的（无末尾的0）输出                Printf("%g", 10.20)                            10.2
%G        根据情况选择 %E 或 %f 以产生更紧凑的（无末尾的0）输出                Printf("%G", 10.20+2i)                        (10.2+2i)
```

复制

字符串与字节切片

```javascript
占位符                        说明                                                举例                                    输出
%s        输出字符串表示（string类型或[]byte)                            Printf("%s", []byte("Go语言中文网"))        Go语言中文网
%q        双引号围绕的字符串，由Go语法安全地转义                            Printf("%q", "Go语言中文网")                "Go语言中文网"
%x        十六进制，小写字母，每字节两个字符                                Printf("%x", "golang")                        676f6c616e67
%X        十六进制，大写字母，每字节两个字符                                Printf("%X", "golang")                        676F6C616E67
```

复制

指针

```javascript
占位符                        说明                                                举例                                    输出
%p        十六进制表示，前缀 0x                                            Printf("%p", &site)                            0x4f57f0
```