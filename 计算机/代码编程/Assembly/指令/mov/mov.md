# mov

复制指令，把一个寄存器/变量的值复制到另一个寄存器/变量

要求：

1.  俩个操作数的大小必须相同
2.  俩个操作数其中一个必须用寄存器中作为中介
3.  指令指针寄存器（ip、rip、rip不能作为目标操作数

格式

```6502&#x20;assembly
MOVE SIZE [M]/R, L/[M]/R
```
