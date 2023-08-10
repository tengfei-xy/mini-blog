# free

## 目录

-   [cache和buffer](#cache和buffer)

命令解释

```bash
[root@bbm ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           7.6G        1.2G        359M         32M        6.1G        6.1G
Swap:          3.9G        1.8M        3.9G
# total：总内存大小。
# used：已经使用的内存大小(这里面包含cached和buffers和shared部分)。
# free：空闲的内存大小。
# shared：进程间共享内存(一般不会用，可以忽略)。
# buffers：内存中写完的东西缓存起来，这样快速响应请求，后面数据再定期刷到磁盘上。
# cached：内存中读完缓存起来内容占的大小(这部分是为了下次查询时快速返回)。
# available：free + buffer/cache - 不可被回收内存(共享内存段、tmpfs、ramfs等)。
# Swap：硬盘上交换分区的使用大小。
```

## cache和buffer

Cache(缓存)，为了调高CPU和内存之间数据交换而设计，Buffer(缓冲)为了提高内存和硬盘(或其他I/O设备的数据交换而设计)。

Cache主要是针对读操作设计的，不过Cache概念可能容易混淆，我理解为CPU本身就有Cache，包括一级缓存、二级缓存、三级缓存，我们知道CPU所有的指令操作对接的都是内存，而CPU的处理能力远高于内存速度，所以为了不让CPU资源闲置，Intel等公司在CPU内部集成了一些Cache，但毕竟不能放太多电路在里面，所以这部分Cache并不是很大，主要是用来存放一些常用的指令和常用数据，真正大部分Cache的数据应该是占用内存的空间来缓存请求过的数据，即上面的Cached部分(这部分纯属个人理解，正确与否有待考证)。

Buffer主要是针对写操作设计的，更细的说是针对内存和硬盘之间的写操作来设计的，目的是将写的操作集中起来进行，减少磁盘碎片和硬盘反复寻址过程，提高性能。

在Linux系统内部有一个守护进程会定期清空Buffer中的内容，将其写入硬盘内，当手动执行sync命令时也会触发上述操作。

free中的-/+ buffers/cache看做两部分：

-buffers/cache：正在使用的内存大小(注意不是used部分，因为buffers和cached并不是正在使用的，组织和人民需要是它们是可以释放的)，其值=used-buffers-cached。

+buffers/cache：可用的内存大小(同理也不是free表示的部分)，其值=free+buffers+cached。

设计的目的就是当上面提到的+buffers/cache表示的可用内存都已使用完，新的读写请求过来后，会把内存中的部分数据写入磁盘，从而把磁盘的部分空间当做虚拟内存来使用。
