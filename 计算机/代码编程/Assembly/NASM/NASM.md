# NASM

## 目录

-   [说明](#说明)
-   [使用](#使用)

# 说明

NASM（Netwide Assembler）是一款广泛使用的汇编语言编译器。它可以将汇编语言源代码转换成可执行的二进制文件或目标文件。NASM是一个开源工具，可以在多个平台上使用，包括Windows、Linux和macOS。

NASM支持多种不同的汇编语法，包括Intel语法和AT\&T语法。它提供了丰富的指令集支持，可以用于编写系统级的低级代码、嵌入式系统和驱动程序等。由于NASM生成的二进制代码具有高度的可移植性和性能，因此它被广泛应用于操作系统开发、嵌入式系统编程和反汇编等领域。

NASM具有灵活的宏汇编功能，允许程序员定义和使用自己的宏指令，以提高代码的可读性和重用性。它还支持包括条件编译、循环和标签等高级语言特性，使得程序员能够更方便地编写复杂的汇编代码。

总之，NASM是一款功能强大的汇编语言编译器，被广泛用于系统级编程和底层软件开发。

# 使用

使用NASM进行汇编：在命令行终端中输入以下命令来使用NASM进行汇编：

```bash
# output_format 指定输出文件的格式，可以是目标文件格式，如`elf`、`macho`、`win32`，或可执行文件格式，如`bin`。具体使用哪种格式取决于你的需求。
# input_file.asm 你的汇编代码文件的名称。
# output_file 生成的目标文件或可执行文件的名称。

nasm -f <output_format> <input_file.asm> -o <output_file>
```

例如，如果你想将汇编代码文件`myfile.asm`汇编为可执行文件`myprogram`，可以使用以下命令：

```bash
nasm -f elf test.asm -o test

# 编译为macOS的可执行64位文件
nasm -f macho64 test.asm -o test
```
