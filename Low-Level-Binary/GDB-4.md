# GDB baby step 4

## Description
main calls a function that multiplies eax by a constant. The flag for this challenge is that constant in decimal base. If the constant you find is 0x1000, the flag will be picoCTF{4096}.

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Binary

## Hints
1. A function can be referenced by either its name or its starting address in gdb.

## Solution
We are given an assembly program. We want to look at it to see what it does so we call, ```(gdb) disassemble main```.
```x86asm
Dump of assembler code for function main:
   0x000000000040111c <+0>:     endbr64 
   0x0000000000401120 <+4>:     push   %rbp
   0x0000000000401121 <+5>:     mov    %rsp,%rbp
   0x0000000000401124 <+8>:     sub    $0x20,%rsp
   0x0000000000401128 <+12>:    mov    %edi,-0x14(%rbp)
   0x000000000040112b <+15>:    mov    %rsi,-0x20(%rbp)
   0x000000000040112f <+19>:    movl   $0x28e,-0x4(%rbp)
   0x0000000000401136 <+26>:    movl   $0x0,-0x8(%rbp)
   0x000000000040113d <+33>:    mov    -0x4(%rbp),%eax
   0x0000000000401140 <+36>:    mov    %eax,%edi
   0x0000000000401142 <+38>:    call   0x401106 <func1>
   0x0000000000401147 <+43>:    mov    %eax,-0x8(%rbp)
   0x000000000040114a <+46>:    mov    -0x4(%rbp),%eax
   0x000000000040114d <+49>:    leave  
   0x000000000040114e <+50>:    ret    
End of assembler dump.
```
It appears to call a function func1, and so we want to see what func1 does, ```(gdb) disassemble func1```
```x86asm
Dump of assembler code for function func1:
   0x0000000000401106 <+0>:     endbr64 
   0x000000000040110a <+4>:     push   %rbp
   0x000000000040110b <+5>:     mov    %rsp,%rbp
   0x000000000040110e <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000401111 <+11>:    mov    -0x4(%rbp),%eax
   0x0000000000401114 <+14>:    imul   $0x3269,%eax,%eax
   0x000000000040111a <+20>:    pop    %rbp
   0x000000000040111b <+21>:    ret    
End of assembler dump.
```
In this function we multiple the eax register by the constant 0x3269, and by the description we convert this to decimal and we have our flag.

## Flag
picoCTF{12905}