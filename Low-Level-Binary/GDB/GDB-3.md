# GDB baby step 3

## Description
Now for something a little different. 0x2262c96b is loaded into memory in the main function. Examine byte-wise the memory that the constant is loaded in by using the GDB command x/4xb addr. The flag is the four bytes as they are stored in memory. If you find the bytes 0x11 0x22 0x33 0x44 in the memory location, your flag would be: picoCTF{0x11223344}.

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Binary

## Hints
1. You'll need to breakpoint the instruction after the memory load.
2. Use the gdb command x/4xb addr with the memory location as the address addr to examine. [GDB manual page](https://ftp.gnu.org/old-gnu/Manuals/gdb/html_node/gdb_55.html).
3. Any registers in addr should be prepended with $ like $rbp.
4. Don't use square brackets for addr
5. What is [endianness](https://en.wikipedia.org/wiki/Endianness)?

## Solution
We are given an assembly program that we again need to allow executable permissions too using: ```$ chmod +x debugger0_c```. Again we need to disassemble just to quickly look over the program.
```(gdb) disassemble main``` gives us: 
```x86asm
Dump of assembler code for function main:
   0x0000000000401106 <+0>:     endbr64 
   0x000000000040110a <+4>:     push   %rbp
   0x000000000040110b <+5>:     mov    %rsp,%rbp
   0x000000000040110e <+8>:     mov    %edi,-0x14(%rbp)
   0x0000000000401111 <+11>:    mov    %rsi,-0x20(%rbp)
   0x0000000000401115 <+15>:    movl   $0x2262c96b,-0x4(%rbp)
   0x000000000040111c <+22>:    mov    -0x4(%rbp),%eax
   0x000000000040111f <+25>:    pop    %rbp
   0x0000000000401120 <+26>:    ret    
End of assembler dump.
```
We notice the eax register at line +22, so we set our breakpoint there, ```(gdb) break *main+22```, then run the program to that point,
```(gdb) run```. We need to see a value in memory as per the description, so we run: ```(gdb) x/4xb $rbp-0x4```, which gives us the four bytes stored in this memory location in little endian.
```
0x7ffd02311abc: 0x6b    0xc9    0x62    0x22
```
These four bytes gives us our flag.

## Flag
picoCTF{0x6bc96222}