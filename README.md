# elf-scratch-hex

## Description

ELF executables written from scratch in hex for learning and fun! There are two executables `hello_elf` and `hello_world` written for Linux x86-64.

## 1. hello_elf

This executable just calls syscall sys_exit with 0. Breakdown of the binary file:

```
ELF header

00000000  7f 45 4c 46 02 01 01 00  00 00 00 00 00 00 00 00  |.ELF............|
          [    1    ] [    2    ]  [          3          ]
00000010  02 00 3e 00 01 00 00 00  78 00 40 00 00 00 00 00  |..>.....x.@.....|
          [ 4 ] [ 5 ] [    6    ]  [          7          ]
00000020  40 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |@...............|
          [          8          ]  [          9          ]
00000030  00 00 00 00 40 00 38 00  01 00 00 00 00 00 00 00  |....@.8.........|
          [    10   ] [11 ] [12 ]  [13 ] [      14       ]

Program header

00000040  01 00 00 00 05 00 00 00  00 00 00 00 00 00 00 00  |................|
          [   15    ] [   16    ]  [         17          ]
00000050  00 00 40 00 00 00 00 00  00 00 00 00 00 00 00 00  |..@.............|
          [          18         ]  [         19          ]
00000060  10 00 00 00 00 00 00 00  10 00 00 00 00 00 00 00  |................|
          [          20         ]  [         21          ]
00000070  00 00 00 00 00 00 00 00  48 c7 c0 3c 00 00 00 48  |........H..<...H|
          [          22         ]  [         23
00000080  c7 c7 00 00 00 00 0f 05                           |........|
                     23         ]
```

```
[1]  ELF magic number
[2]  64 bit, little endian, version, no extension
[3]  Reserved
[4]  ELF executable
[5]  x86-64 architecture
[6]  version
[7]  Virtual address of the entry point
     0x400078: The program is loaded at 0x400000 and the code
               starts at 0x78
[8]  Program header table offset
     0x40: The program header table starts at 64 bytes into
           the file, right after the ELF header
[9]  Section header table offset
     Doesn't exist in this executable
[10] Flags
[11] Size of ELF header (64 bytes)
[12] Size of each entry in Program header table (56 bytes)
[13] Number of Program headers (1)
[14] Metadata about sections (not relevant to this executable)

[15] Type of segment
     This is a loadable segment
[16] Flags
     Read and Execute
[17] Offset
[18] Virtual address of segment
[19] Physical address (not relevant for System V)
[20] Number of bytes in file image of segment (16)
[21] Number of bytes in memory image of segment (16)
[22] Alignment (No alignment)
[23] The code (16 bytes as specified in [20] and [21])
     mov rax, 60      48 C7 C0 3C 00 00 00
     mov rdi, 0       48 C7 C7 00 00 00 00
     syscall          0F 05
```

## 2. hello_world

This executable prints "Hello, World!" using syscall sys_write. Breakdown of the binary file:

```
ELF header start

00000000  7f 45 4c 46 02 01 01 00  00 00 00 00 00 00 00 00  |.ELF............|
00000010  02 00 3e 00 01 00 00 00  78 00 40 00 00 00 00 00  |..>.....x.@.....|
00000020  40 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |@...............|
00000030  00 00 00 00 40 00 38 00  02 00 00 00 00 00 00 00  |....@.8.........|
                                   ^^^^^
Program header 1 start

00000040  01 00 00 00 05 00 00 00  00 00 00 00 00 00 00 00  |................|
          [    1    ] [    2    ]  [          3          ]
00000050  00 00 40 00 00 00 00 00  00 00 00 00 00 00 00 00  |..@.............|
          [           4         ]  [          5          ]
00000060  31 00 00 00 00 00 00 00  31 00 00 00 00 00 00 00  |1.......1.......|
          [           6         ]  [          7          ]
00000070  00 00 00 00 00 00 00 00  48 c7 c0 01 00 00 00 48  |........H......H|
          [           8         ]  [          9
00000080  c7 c7 01 00 00 00 48 be  e1 00 40 00 00 00 00 00  |......H...@.....|
00000090  48 c7 c2 0e 00 00 00 0f  05 48 c7 c0 3c 00 00 00  |H........H..<...|
000000a0  48 c7 c7 00 00 00 00 0f  05                       |H...............|
                      9             ]

Program header 2 start

000000a0                              01 00 00 00 06 00 00  |H...............|
                                      [         10
000000b0  00 00 00 00 00 00 00 00  00 e1 00 40 00 00 00 00  |...........@....|
           ] [          11          ] [          12       
000000c0  00 00 00 00 00 00 00 00  00 0e 00 00 00 00 00 00  |................|
           ] [          13          ] [          14
000000d0  00 0e 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
           ] [          15          ] [          16
000000e0  00 48 65 6c 6c 6f 2c 20  57 6f 72 6c 64 21 00     |.Hello, World!.|
           ] [                   17                   ]     
```

```
ELF header same as in hello_elf (except, here we have two
program headers instead of one)

[1]  This is a loadable segment
[2]  Read and Execute
[3]  Offset
[4]  Virtual address of segment (0x400000)
[5]  Physical address
[6]  File size: 49 bytes
[7]  Memory size: 49 bytes
[8]  Alignment (No alignment)
[9]  The code (49 bytes as specified in [20] and [21])
     mov rax, 1                 48 C7 C0 01 00 00 00
     mov rdi, 1                 48 C7 C7 01 00 00 00
     movabs rsi, 0x4000e1       48 BE E1 00 40 00 00 00 00 00
     mov rdx, 0xe               48 C7 C2 0E 00 00 00
     syscall                    0F 05
     mov rax, 60                48 C7 C0 3C 00 00 00
     mov rdi, 0                 48 C7 C7 00 00 00 00
     syscall                    0F 05

[10] Loadable segment, Read and Write
[11] Offset
[12] Virtual address of segment (0x4000e1)
     This is where the data starts
[13] Physical address
[14] File size: 14 bytes
[15] Memory size: 14 bytes
[16] No alignment
[17] The data (14 bytes as specified in [14] and [15])
     Null terminated string "Hello, World"
```

## Disclaimer

Please don't run any file without completely understanding what it does. The author will not be responsible for any damages.
