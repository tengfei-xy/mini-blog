# EQU

-   赋值
    ```6502&#x20;assembly
    identifier: EQU expression ; numeric operations
    identifier: EQU 0          ; a single value(constant)
    identifier: EQU "text"     ; text can be four characters max(32-bit)
    ```
-   配合当前位置计算器计算内存空间
    ```6502&#x20;assembly
    motd: DB "I'm in the matrix?",O
    len:  EQU ($- motd)
    ```
