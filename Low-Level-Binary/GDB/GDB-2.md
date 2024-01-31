# GDB baby step 2

## Description
Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Binary

## Hints
1. You could calculate eax yourself, or you could set a breakpoint for after the calculcation and inspect eax to let the program do the heavy-lifting for you.
## Solution
We are given an assembly program again. This time we have to run ```$ chmod +x debugger0_b``` as we know we are going to have to run this to Dynamically analyze it. Next we run ```(gdb) disassemble main``` to see what the program does. It loops too many times and that seems like too much to read through:
```x86asm
Dump of assembler code for function main:
   0x0000000000401106 <+0>:     endbr64 
   0x000000000040110a <+4>:     push   %rbp
   0x000000000040110b <+5>:     mov    %rsp,%rbp
   0x000000000040110e <+8>:     mov    %edi,-0x14(%rbp)
   0x0000000000401111 <+11>:    mov    %rsi,-0x20(%rbp)
   0x0000000000401115 <+15>:    movl   $0x1e0da,-0x4(%rbp)
   0x000000000040111c <+22>:    movl   $0x25f,-0xc(%rbp)
   0x0000000000401123 <+29>:    movl   $0x0,-0x8(%rbp)
   0x000000000040112a <+36>:    jmp    0x401136 <main+48>
   0x000000000040112c <+38>:    mov    -0x8(%rbp),%eax
   0x000000000040112f <+41>:    add    %eax,-0x4(%rbp)
   0x0000000000401132 <+44>:    addl   $0x1,-0x8(%rbp)
   0x0000000000401136 <+48>:    mov    -0x8(%rbp),%eax
   0x0000000000401139 <+51>:    cmp    -0xc(%rbp),%eax
   0x000000000040113c <+54>:    jl     0x40112c <main+38>
   0x000000000040113e <+56>:    mov    -0x4(%rbp),%eax
   0x0000000000401141 <+59>:    pop    %rbp
   0x0000000000401142 <+60>:    ret    
End of assembler dump.
```
So we can instead create a break point at the end where the eax register is set, with the line: ```(gdb) break *main+59```. This creates a break point, and to run our program to that point we call ```(gdb) run``` which we can then analyze with the line
```(gdb) info registers eax``` to get out what is in the register: 
```
eax            0x4af4b             307019
```
This gives us our flag.

## Flag
picoCTF{307019}