# 保存flag

一种是用一个整型变量的不同位来表示不同的flag，然后用位操作符来设置或清除某个flag。例如，你可以定义一个变量flags，然后用它的低四位来表示四个不同的状态：第一位表示是否开启，第二位表示是否暂停，第三位表示是否错误，第四位表示是否完成。那么你可以用以下的代码来操作这些flag

```c
// 定义每个flag对应的掩码
#define FLAG_ON 0x01 // 0000 0001
#define FLAG_PAUSE 0x02 // 0000 0010
#define FLAG_ERROR 0x04 // 0000 0100
#define FLAG_DONE 0x08 // 0000 1000

// 定义一个变量flags
int flags = 0;

// 设置某个flag为1（开启）
flags |= FLAG_ON; // flags = flags | FLAG_ON

// 清除某个flag为0（关闭）
flags &= ~FLAG_ERROR; // flags = flags & (~FLAG_ERROR)

// 判断某个flag是否为1（开启）
if (flags & FLAG_PAUSE) {
    // do something
}

// 切换某个flag的状态（取反）
flags ^= FLAG_DONE; // flags = flags ^ FLAG_DONE
```
