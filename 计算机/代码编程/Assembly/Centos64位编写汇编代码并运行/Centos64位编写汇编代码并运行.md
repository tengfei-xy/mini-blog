# Centos64位编写汇编代码并运行

编写文件

```6502&#x20;assembly
section .data
    hello db 'Hello, World!', 0

section .text
    global _start

_start:
    ; 在标准输出中打印字符串
    mov eax, 4
    mov ebx, 1
    mov ecx, hello
    mov edx, 13
    int 0x80

    ; 退出程序
    mov eax, 1
    xor ebx, ebx
    int 0x80
```

生成目标文件

```bash
nasm -g -f elf64 test.asm -o test.o
# 添加-g以让gdb调试
```

将目标文件链接成可执行文件：

```bash
ld  -arch x86_64 -o test test.o
# 如果添加 -s 将无法调试
```

添加可执行权限

```bash
chmod +x ./test
```

执行

```6502&#x20;assembly
./test
```
