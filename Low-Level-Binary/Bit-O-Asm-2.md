# Bit-O-Asm-2

## Description
Can you figure out what is in the eax register? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Binary

## Hints
1. PTR's or 'pointers', reference a location in memory where values can be stored.

## Solution
We are once again given a disassembler dumb:
```
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    mov    eax,DWORD PTR [rbp-0x4]
<+25>:    pop    rbp
<+26>:    ret
```
Here we have to worry about pointers to find the eax register's contents. We know that we want the value from the pointer [rbp-0x4] and we know that from these two lines:
```
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    mov    eax,DWORD PTR [rbp-0x4]
```
The hex value 0x9fe1a is our flag, and we just convert it to decimal.

## Flag
picoCTF{654874}