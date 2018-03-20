# Questions
## Main questions
b. What is the address of the code section?
	.text -> segment 02 -> first LOAD line -> 0x08048000 
c. Where does the heap start?
	.bss -> segment 3 -> second LOAD line -> 0x08049f08 
d. Where does the stack start? Are you sure?
	Segment GNU_STACK -> 0x00000000 -> no, the stack starts at the end and the addresses shrink -> 0xfffdd000 0xffffe000
## Secondary questions
Try this challenge on a 64 bit system.
a. What is the address of the code section?
b. Where does the heap start?
c. Where does the stack start?

Aus <https://exploit.courses/#/challenge/0> 


```
root@hlUbuntu32:~/challenges/challenge00# readelf -l challenge0

Elf file type is EXEC (Executable file)
Entry point 0x8048340
There are 9 program headers, starting at offset 52

Program Headers:
  Type           Offset   VirtAddr   PhysAddr   FileSiz MemSiz  Flg Align
  PHDR           0x000034 0x08048034 0x08048034 0x00120 0x00120 R E 0x4
  INTERP         0x000154 0x08048154 0x08048154 0x00013 0x00013 R   0x1
	  [Requesting program interpreter: /lib/ld-linux.so.2]
  LOAD           0x000000 0x08048000 0x08048000 0x00634 0x00634 R E 0x1000
  LOAD           0x000f08 0x08049f08 0x08049f08 0x00118 0x0011c RW  0x1000
  DYNAMIC        0x000f14 0x08049f14 0x08049f14 0x000e8 0x000e8 RW  0x4
  NOTE           0x000168 0x08048168 0x08048168 0x00044 0x00044 R   0x4
  GNU_EH_FRAME   0x00053c 0x0804853c 0x0804853c 0x0002c 0x0002c R   0x4
  GNU_STACK      0x000000 0x00000000 0x00000000 0x00000 0x00000 RW  0x10
  GNU_RELRO      0x000f08 0x08049f08 0x08049f08 0x000f8 0x000f8 R   0x1

 Section to Segment mapping:
  Segment Sections...
   00
   01     .interp
   02     .interp .note.ABI-tag .note.gnu.build-id .gnu.hash .dynsym .dynstr .gnu.version .gnu.version_r .rel.dyn .rel.plt .init .plt .plt.got .text .fini .rodata .eh_frame_hdr .eh_frame
   03     .init_array .fini_array .jcr .dynamic .got .got.plt .data .bss
   04     .dynamic
   05     .note.ABI-tag .note.gnu.build-id
   06     .eh_frame_hdr
   07
   08     .init_array .fini_array .jcr .dynamic .got




root@hlUbuntu32:~/challenges/challenge00# gdb -q challenge0
Reading symbols from challenge0...(no debugging symbols found)...done.
gdb-peda$ b *main
Breakpoint 1 at 0x804843b
gdb-peda$ run
Starting program: /root/challenges/challenge00/challenge0

 [----------------------------------registers-----------------------------------]
EAX: 0xf7fccddc --> 0xffffd74c --> 0xffffd890 ("SHELL=/bin/bash")
EBX: 0x0
ECX: 0xe4163c04
EDX: 0xffffd6d4 --> 0x0
ESI: 0xf7fcb000 --> 0x1b1db0
EDI: 0xf7fcb000 --> 0x1b1db0
EBP: 0x0
ESP: 0xffffd6ac --> 0xf7e31637 (<__libc_start_main+247>:        add    esp,0x10)
EIP: 0x804843b (<main>: lea    ecx,[esp+0x4])
EFLAGS: 0x292 (carry parity ADJUST zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x8048432 <frame_dummy+34>:  add    esp,0x10
   0x8048435 <frame_dummy+37>:  leave
   0x8048436 <frame_dummy+38>:  jmp    0x80483b0 <register_tm_clones>
=> 0x804843b <main>:    lea    ecx,[esp+0x4]
   0x804843f <main+4>:  and    esp,0xfffffff0
   0x8048442 <main+7>:  push   DWORD PTR [ecx-0x4]
   0x8048445 <main+10>: push   ebp
   0x8048446 <main+11>: mov    ebp,esp
[------------------------------------stack-------------------------------------]
0000| 0xffffd6ac --> 0xf7e31637 (<__libc_start_main+247>:       add    esp,0x10)
0004| 0xffffd6b0 --> 0x1
0008| 0xffffd6b4 --> 0xffffd744 --> 0xffffd868 ("/root/challenges/challenge00/challenge0")
0012| 0xffffd6b8 --> 0xffffd74c --> 0xffffd890 ("SHELL=/bin/bash")
0016| 0xffffd6bc --> 0x0
0020| 0xffffd6c0 --> 0x0
0024| 0xffffd6c4 --> 0x0
0028| 0xffffd6c8 --> 0xf7fcb000 --> 0x1b1db0
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value

Breakpoint 1, 0x0804843b in main ()
gdb-peda$ info proc mappings
process 20343
Mapped address spaces:

		Start Addr   End Addr       Size     Offset objfile
		 0x8048000  0x8049000     0x1000        0x0 /root/challenges/challenge00/challenge0    <- 1a) .text
		 0x8049000  0x804a000     0x1000        0x0 /root/challenges/challenge00/challenge0
		 0x804a000  0x804b000     0x1000     0x1000 /root/challenges/challenge00/challenge0
		0xf7e19000 0xf7fc8000   0x1af000        0x0 /lib/i386-linux-gnu/libc-2.23.so
		0xf7fc8000 0xf7fc9000     0x1000   0x1af000 /lib/i386-linux-gnu/libc-2.23.so
		0xf7fc9000 0xf7fcb000     0x2000   0x1af000 /lib/i386-linux-gnu/libc-2.23.so
		0xf7fcb000 0xf7fcc000     0x1000   0x1b1000 /lib/i386-linux-gnu/libc-2.23.so
		0xf7fcc000 0xf7fcf000     0x3000        0x0
		0xf7fd4000 0xf7fd6000     0x2000        0x0
		0xf7fd6000 0xf7fd8000     0x2000        0x0 [vvar]
		0xf7fd8000 0xf7fd9000     0x1000        0x0 [vdso]
		0xf7fd9000 0xf7ffb000    0x22000        0x0 /lib/i386-linux-gnu/ld-2.23.so
		0xf7ffb000 0xf7ffc000     0x1000        0x0
		0xf7ffc000 0xf7ffd000     0x1000    0x22000 /lib/i386-linux-gnu/ld-2.23.so
		0xf7ffd000 0xf7ffe000     0x1000    0x23000 /lib/i386-linux-gnu/ld-2.23.so
		0xfffdd000 0xffffe000    0x21000        0x0 [stack]
```
