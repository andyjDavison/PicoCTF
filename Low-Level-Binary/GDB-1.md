# GDB baby step 1

## Description
Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Bianry

## Hints
1. gdb is a very good debugger to use for this problem and many others!
2. main is actually a recognized symbol that can be used with gdb commands.

## Solution
We are given an assembly program, and all we have to do is run ```$ gdb debugger0_a``` and we are able to debug the program. For this all we need to do is this command ```(gdb) disassemble main``` and we get this output:
```x86asm
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64 
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret    
End of assembler dump.
```
Then we just convert the hex number that was placed in the eax register to decimal and we have our flag.

## Flag
picoCTF{549698}