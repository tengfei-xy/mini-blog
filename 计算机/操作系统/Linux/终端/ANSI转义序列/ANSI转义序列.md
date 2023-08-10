# ANSI转义序列

诸如`\e[m`或`\e[?1000l`均是ANSI转义序列，这是一种带内信号，用于控制终端上的光标位置、颜色和其他选项。这类序列会解释为相应的指令。在此ANSI序列出现之前的二十世纪七十年代，各个产商使用自己的转移序列。

> windows
> 在微软[Windows 10](https://zh.wikipedia.org/wiki/Windows_10 "Windows 10")更新TH2之前，[Windows操作系统](https://zh.wikipedia.org/wiki/Windows操作系统 "Windows操作系统")的[Win32控制台](https://zh.wikipedia.org/wiki/Win32控制台 "Win32控制台")是不支持ANSI转义序列的。

> VT52
> [VT52](https://zh.wikipedia.org/w/index.php?title=VT52\&action=edit\&redlink=1 "VT52")终端允许通过发送`ESC`字符、`y`字符，后面跟上两个等于x,y位置的数值加上32的字符（这是为了从ASCII空格字符开始，并避开控制字符），将光标置于屏幕上的x,y位置。可见每个终端对于控制屏幕的光标位置的序列都不一样。后来又了VT100。
