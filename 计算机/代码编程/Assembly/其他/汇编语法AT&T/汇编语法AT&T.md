# 汇编语法AT\&T

## 目录

-   [赋值方向](#赋值方向)
-   [操作数前缀](#操作数前缀)
-   [跳转和调用指令](#跳转和调用指令)
-   [内存间接寻址格式](#内存间接寻址格式)
-   [指令后缀](#指令后缀)

## 赋值方向

复制方向从左到右

`movq	%rsp, %rbp`

## 操作数前缀

寄存器需要%；立即数需要\$；标识符直接引用

```6502&#x20;assembly
values: .long 0x5a5a5a5a
movl values,%eax
```

代码段说明：values是标识符变量，这段意思是把values转入eax变量

`movl $balues,%ebx`表示将values变量地址转入ebx寄存器

## 跳转和调用指令

远跳转ljmp：段地址和段内偏移组成

`ljmp $section:$offset`

远调用lcal：段地址和段内偏移组成

`lcal $section:$offse`

远返回lret：无操作数

`lret`

## 内存间接寻址格式

使用()来间接寻址，例如

```6502&#x20;assembly
SECTION:displacement(base,index,scale)
```

section用于指定寄存器；scale默认为1，可取1、2、4、8

## 指令后缀

访问数据时需要指定**操作数**的位宽

`l`表示双字节；`b`表示一个字节；`w`表示一个字；`q`表示四字节

例如`movq %rax,%rbx`

jmp 的地址标识符的后缀——f（forward，向前）；b（back，向后）

例如：

`jmp 1f`
