# Questions
	a) In which section and segment is the variable globalStaticVariable stored?
	b) In which section and segment is the variable heapVar stored?
	c) In which section and segment is the variable localStackVar stored?
	d) Can you locate the content of the variables in the ELF binary? If not, why not?

Aus <https://exploit.courses/#/challenge/1> 

## Try 1

```
root@hlUbuntu32:~/challenges/challenge01# ./challenge1
	Global variable:        0x804a020      -> .data -> 2. LOAD  (vergleichen mit Addr unten bei Section Headers)
	Global static variable: 0x8048540      -> .rodata -> 1. LOAD
	Stack variable:         0x8048550      -> .rodata -> 1. LOAD
	Heap variable:          0x91b8008      -> variable  -> GNU_STACK     TODO: häääää, warum heap in stack und nicht die stack variable??


root@hlUbuntu32:~/challenges/challenge01# readelf -l -S challenge1
	There are 31 section headers, starting at offset 0x1854:
	
	Section Headers:
	  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
	  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
	  [ 1] .interp           PROGBITS        08048154 000154 000013 00   A  0   0  1
	  [ 2] .note.ABI-tag     NOTE            08048168 000168 000020 00   A  0   0  4
	  [ 3] .note.gnu.build-i NOTE            08048188 000188 000024 00   A  0   0  4
	  [ 4] .gnu.hash         GNU_HASH        080481ac 0001ac 000020 04   A  5   0  4
	  [ 5] .dynsym           DYNSYM          080481cc 0001cc 000060 10   A  6   1  4
	  [ 6] .dynstr           STRTAB          0804822c 00022c 000053 00   A  0   0  1
	  [ 7] .gnu.version      VERSYM          08048280 000280 00000c 02   A  5   0  2
	  [ 8] .gnu.version_r    VERNEED         0804828c 00028c 000020 00   A  6   1  4
	  [ 9] .rel.dyn          REL             080482ac 0002ac 000008 08   A  5   0  4
	  [10] .rel.plt          REL             080482b4 0002b4 000018 08  AI  5  24  4
	  [11] .init             PROGBITS        080482cc 0002cc 000023 00  AX  0   0  4
	  [12] .plt              PROGBITS        080482f0 0002f0 000040 04  AX  0   0 16
	  [13] .plt.got          PROGBITS        08048330 000330 000008 00  AX  0   0  8
	  [14] .text             PROGBITS        08048340 000340 0001e2 00  AX  0   0 16
	  [15] .fini             PROGBITS        08048524 000524 000014 00  AX  0   0  4
	  [16] .rodata           PROGBITS        08048538 000538 000091 00   A  0   0  4
	  [17] .eh_frame_hdr     PROGBITS        080485cc 0005cc 00002c 00   A  0   0  4
	  [18] .eh_frame         PROGBITS        080485f8 0005f8 0000cc 00   A  0   0  4
	  [19] .init_array       INIT_ARRAY      08049f08 000f08 000004 00  WA  0   0  4
	  [20] .fini_array       FINI_ARRAY      08049f0c 000f0c 000004 00  WA  0   0  4
	  [21] .jcr              PROGBITS        08049f10 000f10 000004 00  WA  0   0  4
	  [22] .dynamic          DYNAMIC         08049f14 000f14 0000e8 08  WA  6   0  4
	  [23] .got              PROGBITS        08049ffc 000ffc 000004 04  WA  0   0  4
	  [24] .got.plt          PROGBITS        0804a000 001000 000018 04  WA  0   0  4
	  [25] .data             PROGBITS        0804a018 001018 000012 00  WA  0   0  4
	  [26] .bss              NOBITS          0804a02a 00102a 000002 00  WA  0   0  1
	  [27] .comment          PROGBITS        00000000 00102a 000034 01  MS  0   0  1
	  [28] .shstrtab         STRTAB          00000000 00174a 00010a 00      0   0  1
	  [29] .symtab           SYMTAB          00000000 001060 000480 10     30  47  4
	  [30] .strtab           STRTAB          00000000 0014e0 00026a 00      0   0  1
	Key to Flags:
	  W (write), A (alloc), X (execute), M (merge), S (strings)
	  I (info), L (link order), G (group), T (TLS), E (exclude), x (unknown)
	  O (extra OS processing required) o (OS specific), p (processor specific)
	
	Elf file type is EXEC (Executable file)
	Entry point 0x8048340
	There are 9 program headers, starting at offset 52
	
	Program Headers:
	  Type           Offset   VirtAddr   PhysAddr   FileSiz MemSiz  Flg Align
	  PHDR           0x000034 0x08048034 0x08048034 0x00120 0x00120 R E 0x4
	  INTERP         0x000154 0x08048154 0x08048154 0x00013 0x00013 R   0x1
		  [Requesting program interpreter: /lib/ld-linux.so.2]
	  LOAD           0x000000 0x08048000 0x08048000 0x006c4 0x006c4 R E 0x1000
	  LOAD           0x000f08 0x08049f08 0x08049f08 0x00122 0x00124 RW  0x1000
	  DYNAMIC        0x000f14 0x08049f14 0x08049f14 0x000e8 0x000e8 RW  0x4
	  NOTE           0x000168 0x08048168 0x08048168 0x00044 0x00044 R   0x4
	  GNU_EH_FRAME   0x0005cc 0x080485cc 0x080485cc 0x0002c 0x0002c R   0x4
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
```

## Try 2
```
root@hlUbuntu32:~/challenges/challenge01# ./challenge1
	Global variable:        0x804a024      -> .data
	Global static variable: 0x8048610      -> .rodata
	Stack variable:         0xffa03b4c     -> stack
	Heap variable:          0x9b36008      -> .bss
	Function:               0x804849b      -> .text

root@hlUbuntu32:~/challenges/challenge01# readelf -l -S challenge1
	There are 31 section headers, starting at offset 0x18a0:
	
	Section Headers:
	  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
	  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
	  [ 1] .interp           PROGBITS        08048154 000154 000013 00   A  0   0  1
	  [ 2] .note.ABI-tag     NOTE            08048168 000168 000020 00   A  0   0  4
	  [ 3] .note.gnu.build-i NOTE            08048188 000188 000024 00   A  0   0  4
	  [ 4] .gnu.hash         GNU_HASH        080481ac 0001ac 000020 04   A  5   0  4
	  [ 5] .dynsym           DYNSYM          080481cc 0001cc 000070 10   A  6   1  4
	  [ 6] .dynstr           STRTAB          0804823c 00023c 00006e 00   A  0   0  1
	  [ 7] .gnu.version      VERSYM          080482aa 0002aa 00000e 02   A  5   0  2
	  [ 8] .gnu.version_r    VERNEED         080482b8 0002b8 000030 00   A  6   1  4
	  [ 9] .rel.dyn          REL             080482e8 0002e8 000008 08   A  5   0  4
	  [10] .rel.plt          REL             080482f0 0002f0 000020 08  AI  5  24  4
	  [11] .init             PROGBITS        08048310 000310 000023 00  AX  0   0  4
	  [12] .plt              PROGBITS        08048340 000340 000050 04  AX  0   0 16
	  [13] .plt.got          PROGBITS        08048390 000390 000008 00  AX  0   0  8
	  [14] .text             PROGBITS        080483a0 0003a0 000252 00  AX  0   0 16
	  [15] .fini             PROGBITS        080485f4 0005f4 000014 00  AX  0   0  4
	  [16] .rodata           PROGBITS        08048608 000608 0000a9 00   A  0   0  4
	  [17] .eh_frame_hdr     PROGBITS        080486b4 0006b4 000034 00   A  0   0  4
	  [18] .eh_frame         PROGBITS        080486e8 0006e8 0000ec 00   A  0   0  4
	  [19] .init_array       INIT_ARRAY      08049f08 000f08 000004 00  WA  0   0  4
	  [20] .fini_array       FINI_ARRAY      08049f0c 000f0c 000004 00  WA  0   0  4
	  [21] .jcr              PROGBITS        08049f10 000f10 000004 00  WA  0   0  4
	  [22] .dynamic          DYNAMIC         08049f14 000f14 0000e8 08  WA  6   0  4
	  [23] .got              PROGBITS        08049ffc 000ffc 000004 04  WA  0   0  4
	  [24] .got.plt          PROGBITS        0804a000 001000 00001c 04  WA  0   0  4
	  [25] .data             PROGBITS        0804a01c 00101c 000012 00  WA  0   0  4
	  [26] .bss              NOBITS          0804a02e 00102e 000002 00  WA  0   0  1
	  [27] .comment          PROGBITS        00000000 00102e 000034 01  MS  0   0  1
	  [28] .shstrtab         STRTAB          00000000 001793 00010a 00      0   0  1
	  [29] .symtab           SYMTAB          00000000 001064 0004a0 10     30  47  4
	  [30] .strtab           STRTAB          00000000 001504 00028f 00      0   0  1
	Key to Flags:
	  W (write), A (alloc), X (execute), M (merge), S (strings)
	  I (info), L (link order), G (group), T (TLS), E (exclude), x (unknown)
	  O (extra OS processing required) o (OS specific), p (processor specific)
	
	Elf file type is EXEC (Executable file)
	Entry point 0x80483a0
	There are 9 program headers, starting at offset 52
	
	Program Headers:
	  Type           Offset   VirtAddr   PhysAddr   FileSiz MemSiz  Flg Align
	  PHDR           0x000034 0x08048034 0x08048034 0x00120 0x00120 R E 0x4
	  INTERP         0x000154 0x08048154 0x08048154 0x00013 0x00013 R   0x1
		  [Requesting program interpreter: /lib/ld-linux.so.2]
	  LOAD           0x000000 0x08048000 0x08048000 0x007d4 0x007d4 R E 0x1000
	  LOAD           0x000f08 0x08049f08 0x08049f08 0x00126 0x00128 RW  0x1000
	  DYNAMIC        0x000f14 0x08049f14 0x08049f14 0x000e8 0x000e8 RW  0x4
	  NOTE           0x000168 0x08048168 0x08048168 0x00044 0x00044 R   0x4
	  GNU_EH_FRAME   0x0006b4 0x080486b4 0x080486b4 0x00034 0x00034 R   0x4
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
```
