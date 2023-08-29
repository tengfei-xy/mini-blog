# GDB

-   进行汇编
    ```bash
    nasm -g -f elf64 test.asm -o test.o
    # 添加-g以让gdb调试
    ```
    将目标文件链接成可执行文件：
    ```bash
    ld  -arch x86_64 -o test test.o
    # 如果添加 -s 将无法调试
    ```
-   设置断点
    -   根据文件行设置断点
        ```bash
        (gdb) break test.asm:13
        ```
    -   根据函数设置
        ```bash
        (gdb) break _start
        ```
-   开始运行
    ```bash
    (gdb) run
    ```
-   查看寄存器内存状态

    所有寄存器
    ```bash
    (gdb) info registers
    ```
    指定寄存器
    ```bash
    (gdb) print $eax

    ```
    指定寄存器，以十六进制查看
    ```bash
    (gdb) print/x $eax
    ```
