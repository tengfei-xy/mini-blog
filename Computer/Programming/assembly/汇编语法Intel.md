## 书写格式

关键词大写

## 赋值方向

复制方向从右到左

`ADD 目的操作数，源操作数`

## 操作数前缀

寄存器和立即数不需要前缀

## 跳转和调用指令

远跳转JMP：段地址和段内偏移组成

`JMP FAR SECTION:OFFSET`

远调用CALL：段地址和段内偏移组成

`CALL FAR SECTION:OFFSET`

远返回RET：无操作数

`RET`



## 内存间接寻址格式

使用[]来间接寻址，例如

```assembly
SECTION:[base+index*scale+displacement]
```

section用于指定寄存器；scale默认为1，可取1、2、4、8

## 指令后缀

使用内存操作数时应该借助**PTR**限定**操作数的位宽**

例如`MOV EAX,DWORD PTR [EBX]`

其中`DOWRD PTR`表示双字节；`BYTE PTR`表示一个字节；`WORD PTR`表示一个字