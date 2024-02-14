# Bit-O-Asm-4

## Description
Can you figure out what is in the eax register? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Binary

## Hints
1. Don't tell anyone I told you this, but you can solve this problem without understanding the compare/jump relationship.
2. Of course, if you're really good, you'll only need one attempt to solve this problem.

## Solution
We are given a disassembler dump:
```s
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    cmp    DWORD PTR [rbp-0x4],0x2710
<+29>:    jle    0x55555555514e <main+37>
<+31>:    sub    DWORD PTR [rbp-0x4],0x65
<+35>:    jmp    0x555555555152 <main+41>
<+37>:    add    DWORD PTR [rbp-0x4],0x65
<+41>:    mov    eax,DWORD PTR [rbp-0x4]
<+44>:    pop    rbp
<+45>:    ret
```
We need to find out what the final value of the 0x4 pointer is. It is intially set to 0x9fe1a, and here is what it's compared to: ```<+22>:    cmp    DWORD PTR [rbp-0x4],0x2710```. We know 0xfe1a > 0x2710 so we skip the ```jle``` and go to this line ```<+31>:    sub    DWORD PTR [rbp-0x4],0x65```. We subtract 0x65 from 0xfe1a, then immeadiately after jump to line 41, where we get our eax register. Convert the hex math we just did to decimal and we get our flag.

## Flag
picoCTF{654773}