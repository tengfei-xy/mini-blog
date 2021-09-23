# DISCARDFILE | NODISCARDFILE

## Description

使用`DISCARDFILE`参数执行以下操作：

- 自定义丢弃文件的名称、位置、大小和写入模式。默认情况下，每当通过GGSCI使用`START`命令启动进程时，都会生成丢弃文件。要保留默认属性，不需要`DISCARDFILE`参数。
- 指定使用丢弃文件进行处理方法，如果进程从操作系统的命令行开始，并且默认情况下不会创建丢弃文件。

使用`NODISCARDFILE`参数禁用丢弃文件的使用。如果`NODISCARDFILE`与`DISCARDFILE`一起使用，则该过程将放弃。

使用`DISCARDFILE`时，请使用`PURGE`或`APPEND`选项。否则，您必须在开始每个进程运行之前指定不同的丢弃文件名，因为如果没有这些指令之一，甲骨文GoldenGate不会写入现有的丢弃文件，并将终止。

请参阅[“DISCARDROLLOVER”，](https://docs.oracle.com/goldengate/1212/gg-winux/GWURF/gg_parameters045.htm#i1005926)了解如何控制丢弃文件滚动到新文件的频率。

有关丢弃文件的更多信息，请参阅[*管理Windows和UNIX版Oracle GoldenGate*](https://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_errorhandling.htm#GWUAD497)。

默认

如果进程在GGSCI中使用`START`命令启动，默认生成丢弃文件如下：

- 文件以创建文件的进程命名，扩展名为`.dsc`。如果进程是协调的复制品，则每个线程生成一个文件。每个文件名都附加了相应线程的线程ID。
- 该文件是在Oracle GoldenGate安装目录的`dirrpt`子目录中创建的。
- 最大文件大小为50兆字节。
- 在启动时，如果存在丢弃文件，则在编写新数据之前将其清除。

如果进程是从操作系统的命令行启动的，默认情况下不要生成丢弃文件。

语法

```
DISCARDFILE { [file_name]
[, APPEND | PURGE]
[, MAXBYTES n | MEGABYTES n] } |
NODISCARDFILE
```

- `DISCARDFILE`

  指示正在更改丢弃文件的名称或其他属性。

- `file_name`

  丢弃文件的相对名称或完全限定的名称，包括实际文件名。对于协调的复制品，指定最多五个字符的文件名，因为每个文件名都附加了写入它的线程的线程ID。要将文件存储在Oracle GoldenGate目录中，相对路径名称就足够了，因为Oracle GoldenGate将名称与Oracle GoldenGate安装目录限定。

- `APPEND`

  如果文件已经存在，则向现有内容添加新内容。如果既不使用`APPEND`也不使用`PURGE`，您必须在开始每个进程运行之前指定不同的丢弃文件名。

- `PURGE`

  在编写新内容之前，请清除文件。如果既不使用`PURGE`也不使用`APPEND`，您必须在开始每个进程运行之前指定不同的丢弃文件名。

- `MAXBYTES` `n`

  以字节为单位设置文件的最大大小。有效范围为1到2147483646。默认值为50000000。如果超过指定大小，则进程将进行。

- `MEGABYTES` `n`

  以兆字节为单位设置文件的最大大小。有效范围为1到2147。默认值为50 MB。如果超过指定大小，则进程将进行。

- `NODISCARDFILE`

  防止进程创建丢弃文件。



## Example

- Example 1  

  This example specifies a non-default file name and extension, non-default write mode, and non-default maximum file size. This example shows how you could change the default properties of a discard file for an online (started through GGSCI) process or specify the use of a discard file for a process that starts from the command line of the operating system and has no discard file by default.

   ```
   DISCARDFILE .dirrpt/discard.txt, APPEND, MEGABYTES 20
   ```

- Example 2  

  This example changes only the write mode of the default discard file for an online process (started through GGSCI).

  ```
  DISCARDFILE .dirrpt/finance.dsc, APPEND
  ```

- Example 3  

  This example disables the use of a discard file for an online process (started through GGSCI).

  ```
  NODISCARDFILE
  ```

  