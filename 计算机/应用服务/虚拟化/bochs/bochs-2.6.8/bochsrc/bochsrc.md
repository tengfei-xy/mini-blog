# bochsrc

plugin\_ctrl: unmapped=1, biosdev=1, speaker=1, extfpuirq=1, parallel=1, serial=1, iodebug=1
config\_interface: textconfig
display\_library: x
romimage: file=$BXSHARE/BIOS-bochs-latest
vgaromimage: file=$BXSHARE/VGABIOS-lgpl-latest
boot: floppy
floppy\_bootsig\_check: disabled=0
floppya: 1\_44=/dev/rfd0a, status=inserted, write\_protected=0
ata0: enabled=1, ioaddr1=0x1f0, ioaddr2=0x3f0, irq=14
ata0-master: type=none
ata0-slave: type=none
ata1: enabled=1, ioaddr1=0x170, ioaddr2=0x370, irq=15
ata1-master: type=none
ata1-slave: type=none
ata2: enabled=0
ata3: enabled=0
pci: enabled=1, chipset=i440fx
vga: extension=vbe, update\_freq=5
cpu: count=1:1:1, ips=4000000, quantum=16,model=corei7\_haswell\_4770,reset\_on\_triple\_fault=1, cpuid\_limit\_winnt=0, ignore\_bad\_msrs=1,mwait\_is\_nop=0, msrs="msrs.def"

cpuid: x86\_64=1,level=6, mmx=1, sep=1, simd=avx512, aes=1, movbe=1, xsave=1,apic=x2apic,sha=1,movbe=1,adx=1,xsaveopt=1,avx\_f16c=1,avx\_fma=1,bmi=bmi24,1g\_pages=1,pcid=1,fsgsbase=1,smep=1,smap=1,mwait=1,vmx=1
cpuid: family=6, model=0x1a, stepping=5, vendor\_string= "GenuineIntel",brand\_string="Intel(R) Core(TM) i7-4770 CPU (Haswell)"

print\_timestamps: enabled=0
port\_e9\_hack: enabled=0
magic\_break: enabled=0
clock: sync=none, time0=local,rtc\_sync=0
log: -
logprefix: %t%e%d
debug: action=ignore
info: action=ignore
error: action=ignore
panic: action=ask
keyboard: type=mf, serial\_delay=250, paste\_delay=100000, user\_shortcut=none
mouse: type=ps2, enabled=0, toggle=ctrl+mbutton
speaker: enabled=1, mode=system
parport1: enabled=1, file=none
parport2: enabled=0
com1: enabled=1, mode=null
com2: enabled=0
com3: enabled=0
com4: enabled=0
