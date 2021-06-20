# BIOS 中断向量表

| 中断                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| INT 00h                                                      | CPU：除零错，或商不合法时触发                                |
| INT 01h                                                      | CPU：单步陷阱，TF标记为打开状态时，每条指令执行后触发        |
| INT 02h                                                      | CPU：[非可屏蔽中断](https://zh.wikipedia.org/w/index.php?title=非可屏蔽中断&action=edit&redlink=1)，如[引导自我测试](https://zh.wikipedia.org/wiki/開機自我測試)时发生内存错误。 |
| INT 03h                                                      | CPU：第一个未定义的中断向量，约定俗成仅用于调试程序          |
| INT 04h                                                      | CPU：算数溢出。通常由INTO指令在置溢出位时触发。              |
| INT 05h                                                      | 在按下Shift-[Print Screen](https://zh.wikipedia.org/wiki/Print_Screen)或BOUND指令检测到范围异常时触发。 |
| INT 06h                                                      | CPU：非法指令。                                              |
| INT 07h                                                      | CPU：没有[数学协处理器](https://zh.wikipedia.org/wiki/8087协处理器)时尝试执行浮点指令触发。 |
| INT 08h                                                      | IRQ0：可编程中断控制器每 55 毫秒触发一次，即每秒 18.2 次。   |
| INT 09h                                                      | IRQ1：每次键盘按下、按住、释放。                             |
| INT 0Ah                                                      | IRQ2：                                                       |
| INT 0Bh                                                      | IRQ3：[COM2/COM4](https://zh.wikipedia.org/wiki/串口)。      |
| INT 0Ch                                                      | IRQ4：[COM1/COM3](https://zh.wikipedia.org/wiki/串口)。      |
| INT 0Dh                                                      | IRQ5：硬盘控制器（PC/XT 下）或 [LPT2](https://zh.wikipedia.org/wiki/并口)。 |
| INT 0Eh                                                      | IRQ6：需要时由[软盘控制器](https://zh.wikipedia.org/wiki/軟碟控制器)调用。 |
| INT 0Fh                                                      | IRQ7：[LPT1](https://zh.wikipedia.org/wiki/并口)。           |
| [INT 10](https://zh.wikipedia.org/wiki/INT_10)h              | 显示服务 - 由BIOS或操作系统设定以供软件调用。<br />AH=00h设定显示模式<br />AH=01h设定游标形态<br />AH=02h设置光标位置，DH光标列数，DL光标行数，BH页码<br />AH=03h获取光标位置与形态<br />AH=04h获取光标位置<br />AH=05h设置显示页<br />AH=06h清除或滚动栏画面(上)<br />AH=07h清除或滚动栏画面(下)<br />AH=08h读取游标处字符与属性<br />AH=09h更改游标处字符与属性<br />AH=0Ah更改游标处字符<br />AH=0Bh设定边界颜色<br />AH=0Eh在TTY模式下写字符<br />AH=0Fh获取当前显示模式<br />AH=13h写字符串 |
| INT 11h                                                      | 返回设备列表。                                               |
| INT 12h                                                      | 获取常规内存容量。                                           |
| [INT 13](https://zh.wikipedia.org/w/index.php?title=INT_13&action=edit&redlink=1)h | 低级磁盘服务。<br />AH=00h复位磁盘驱动器<br />AH=01h检查磁盘驱动器状态<br />AH=02h读扇区<br />AH=03h写扇区<br />AH=04h校验扇区<br />AH=05h格式化磁道<br />AH=08h获取驱动器参数<br />AH=09h初始化硬盘驱动器参数<br />AH=0Ch寻道<br />AH=0Dh复位硬盘控制器<br />AH=15h获取驱动器类型<br />AH=16h获取软驱中盘片的状态 |
| INT 14h                                                      | 串口通信例程。AH=00h初始化串口。AH=01h写出字符。AH=02h读入字符。AH=03h状态 |
| INT 15h                                                      | 其它（系统支持例程）<br />AH=4FH键盘拦截<br />AH=83H事件等待<br />AH=84H读游戏杆<br />AH=85HSysRq 键<br />AH=86H等待<br />AH=87H块移动<br />AH=88H获取扩展内存容量<br />AH=C0H获取系统参数<br />AH=C1H获取扩展 BIOS 数据区块<br />AH=C2H指针设备功能<br />AH=E8h, AL=01h (AX = E801h)获取扩展内存容量（自从 1994 年引入的新功能），可获取到 64MB 以上的内存容量AH=E8h, AL=20h (AX = E820h)查询系统地址映射，该功能取代了 AX=E801h 和 AH=88h |
| INT 16h                                                      | 键盘通信例程<br />AH=00h读字符<br />AH=01h读输入状态<br />AH=02h读 Shift 键（修改键）状态<br />AH=10h读字符（增强版）<br />AH=11h读输入状态（增强版）<br />AH=12h读 Shift 键（修改键）状态（增强版） |
| INT 17h                                                      | 打印服务。AH=00h打印字符。AH=01h初始化打印机。AH=02h检查打印机状态。 |
| INT 18h                                                      | 执行磁带上的 BASIC 程序：“真正的”IBM 兼容机在 ROM 里内置 BASIC 程序，当引导失败时由 BIOS 调用此例程解释执行。（例：打印“Boot disk error. Replace disk and press any key to continue...”这类提示信息） |
| INT 19h                                                      | [加电自检](https://zh.wikipedia.org/wiki/加电自检)之后加载操作系统。 |
| INT 1Ah                                                      | 实时钟服务<br />AH=00h读取实时钟<br />AH=01h设置实时钟<br />AH=02h读取实时钟时间<br />AH=03h设置实时钟时间<br />AH=04h读取实时钟日期<br />AH=05h设置实时钟日期<br />AH=06h设置实时钟闹铃<br />AH=07h重置实时钟闹铃 |
| INT 1Bh                                                      | Ctrl+Break，由 IRQ 9 自动调用。                              |
| INT 1Ch                                                      | 预留，由 IRQ 8 自动调用。                                    |
| INT 1Dh                                                      | 不可调用：指向视频参数表（包含视频模式的数据）的指针。       |
| INT 1Eh                                                      | 不可调用：指向软盘模式表（包含关于软驱的大量信息）的指针。   |
| INT 1Fh                                                      | 不可调用：指向视频图形字符表（包含从 80h 到 FFh 的 [ASCII](https://zh.wikipedia.org/wiki/EASCII) 字符的数据）的信息。 |
| INT 41h                                                      | 地址指针：硬盘参数表（第一硬盘）。                           |
| INT 46h                                                      | 地址指针：硬盘参数表（第二硬盘）。                           |
| INT 4Ah                                                      | 实时钟在闹铃时调用。                                         |
| INT 70h                                                      | IRQ8：由实时钟调用。                                         |
| INT 74h                                                      | IRQ12：由鼠标调用                                            |
| INT 75h                                                      | IRQ13：由数学协处理器调用。                                  |
| INT 76h                                                      | IRQ14：由第一个 IDE 控制器所调用                             |
| INT 77h                                                      | IRQ15：由第二个 IDE 控制器所调用                             |