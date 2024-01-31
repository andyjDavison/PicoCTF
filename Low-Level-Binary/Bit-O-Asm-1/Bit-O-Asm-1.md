# Bit-O-Asm-1

## Description
Can you figure out what is in the eax register? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Binary

## Hints
1. As with most assembly, there is a lot of noise in the instruction dump. Find the one line that pertains to this question and don't second guess yourself!

## Solution
We are given an assembly dump, which looks like this:
```
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x4],edi
<+11>:    mov    QWORD PTR [rbp-0x10],rsi
<+15>:    mov    eax,0x30
<+20>:    pop    rbp
<+21>:    ret
```
The important piece to look at is 
```<+15>:    mov    eax,0x30``` where we know that the hex number 0x30 was moved into the eax register. Convert the hex to decimal and we have the flag.

## Flag
picoCTF{48}
