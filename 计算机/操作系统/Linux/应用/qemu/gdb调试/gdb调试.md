# gdb调试

命令参考文档：[链接](https://qemu-project.gitlab.io/qemu/system/gdb.html?highlight=debug "链接")

```纯文本
qemu-system-x86_64 -kernel other/boot_floppy.img -s -S

# -s 监听端口1234
```

```纯文本
(gdb) target remote | qemu-system-x86_64 -drive file="/Users/tengfei/Documents/github/Melta/other/boot_floppy.img",index=0,format=raw,if=floppy -gdb stdio
```
