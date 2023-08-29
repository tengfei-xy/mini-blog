# bochs-2.6.11编译

## 目录

-   [编译依赖](#编译依赖)
-   [编译参数](#编译参数)

## 编译依赖

Centos:

```bash
yum install glibc-headers gcc-c++
```

ubuntu

```bash
apt-get install build-essential g++ gnome-devel++
```

## 编译参数

```纯文本
./configure --prefix=/usr/local/bochs-2.6.11  --enable-static=PKGS --enable-shared=PKGS --enable-fast-install=PKGS --enable-idle-hack --enable-a20-pin --enable-x86-64 --enable-smp --enable-cpu-level=6 --enable-long-phy-address --enable-large-ramfile --enable-ne2000 --enable-pci  --enable-usb --enable-usb-ohci --enable-usb-xhci --enable-pnic --enable-e1000 --enable-repeat-speedups --enable-fast-function-calls --enable-handlers-chaining --enable-configurable-msrs --enable-show-ips --enable-cpp --enable-debugger --enable-disasm --enable-debugger-gui --enable-iodebug --enable-all-optimizations --enable-readline --enable-logging --enable-assert-checks --enable-raw-serial --enable-clgd54xx --enable-fpu --enable-vmx --enable-svm --enable-3dnow --enable-alignment-check --enable-misaligned-sse --enable-monitor-mwait --enable-avx --enable-x86-debugger --enable-cdrom --enable-sb16 --enable-es1370 --enable-gameport  --enable-xpm
```

注意：相对全部enable参数，缺少--enable-pcidev， --enable-gdb-stub（和--enable-debugger相对）, --enable-docbook, --enable-FEATURE=ARG, --enable-apic, --enable-x2apic, --enable-vbe, --enable-acpi, --enable-trace-cache, --enable-instrumentation
