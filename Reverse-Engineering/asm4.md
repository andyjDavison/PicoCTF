# asm4

## Description
What will asm4("picoCTF_f97bb") return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format.

## Challenge Information
**Point Value:** 400  
**Category:** Reverse Engineering

## Hints
1. Treat the Array argument as a pointer

## Solution
We are given an assembly program, and honestly I can already tell it's gonna be a bitch to go through by hand.
```s
asm4:
        <+0>:   push   ebp
        <+1>:   mov    ebp,esp
        <+3>:   push   ebx
        <+4>:   sub    esp,0x10
        <+7>:   mov    DWORD PTR [ebp-0x10],0x27a
        <+14>:  mov    DWORD PTR [ebp-0xc],0x0
        <+21>:  jmp    0x518 <asm4+27>
        <+23>:  add    DWORD PTR [ebp-0xc],0x1
        <+27>:  mov    edx,DWORD PTR [ebp-0xc]
        <+30>:  mov    eax,DWORD PTR [ebp+0x8]
        <+33>:  add    eax,edx
        <+35>:  movzx  eax,BYTE PTR [eax]
        <+38>:  test   al,al
        <+40>:  jne    0x514 <asm4+23>
        <+42>:  mov    DWORD PTR [ebp-0x8],0x1
        <+49>:  jmp    0x587 <asm4+138>
        <+51>:  mov    edx,DWORD PTR [ebp-0x8]
        <+54>:  mov    eax,DWORD PTR [ebp+0x8]
        <+57>:  add    eax,edx
        <+59>:  movzx  eax,BYTE PTR [eax]
        <+62>:  movsx  edx,al
        <+65>:  mov    eax,DWORD PTR [ebp-0x8]
        <+68>:  lea    ecx,[eax-0x1]
        <+71>:  mov    eax,DWORD PTR [ebp+0x8]
        <+74>:  add    eax,ecx
        <+76>:  movzx  eax,BYTE PTR [eax]
        <+79>:  movsx  eax,al
        <+82>:  sub    edx,eax
        <+84>:  mov    eax,edx
        <+86>:  mov    edx,eax
        <+88>:  mov    eax,DWORD PTR [ebp-0x10]
        <+91>:  lea    ebx,[edx+eax*1]
        <+94>:  mov    eax,DWORD PTR [ebp-0x8]
        <+97>:  lea    edx,[eax+0x1]
        <+100>: mov    eax,DWORD PTR [ebp+0x8]
        <+103>: add    eax,edx
        <+105>: movzx  eax,BYTE PTR [eax]
        <+108>: movsx  edx,al
        <+111>: mov    ecx,DWORD PTR [ebp-0x8]
        <+114>: mov    eax,DWORD PTR [ebp+0x8]
        <+117>: add    eax,ecx
        <+119>: movzx  eax,BYTE PTR [eax]
        <+122>: movsx  eax,al
        <+125>: sub    edx,eax
        <+127>: mov    eax,edx
        <+129>: add    eax,ebx
        <+131>: mov    DWORD PTR [ebp-0x10],eax
        <+134>: add    DWORD PTR [ebp-0x8],0x1
        <+138>: mov    eax,DWORD PTR [ebp-0xc]
        <+141>: sub    eax,0x1
        <+144>: cmp    DWORD PTR [ebp-0x8],eax
        <+147>: jl     0x530 <asm4+51>
        <+149>: mov    eax,DWORD PTR [ebp-0x10]
        <+152>: add    esp,0x10
        <+155>: pop    ebx
        <+156>: pop    ebp
        <+157>: ret
```
So we need to modify it to a runnable assembly program:
```s
.intel_syntax noprefix
    .global asm4

asm4:
                push   ebp
                mov    ebp,esp
                push   ebx
                sub    esp,0x10
                mov    DWORD PTR [ebp-0x10],0x27a
                mov    DWORD PTR [ebp-0xc],0x0
                jmp    jump1
        jump2:  add    DWORD PTR [ebp-0xc],0x1
        jump1:  mov    edx,DWORD PTR [ebp-0xc]
                mov    eax,DWORD PTR [ebp+0x8]
                add    eax,edx
                movzx  eax,BYTE PTR [eax]
                test   al,al
                jne    jump2
                mov    DWORD PTR [ebp-0x8],0x1
                jmp    jump3
        jump4:  mov    edx,DWORD PTR [ebp-0x8]
                mov    eax,DWORD PTR [ebp+0x8]
                add    eax,edx
                movzx  eax,BYTE PTR [eax]
                movsx  edx,al
                mov    eax,DWORD PTR [ebp-0x8]
                lea    ecx,[eax-0x1]
                mov    eax,DWORD PTR [ebp+0x8]
                add    eax,ecx
                movzx  eax,BYTE PTR [eax]
                movsx  eax,al
                sub    edx,eax
                mov    eax,edx
                mov    edx,eax
                mov    eax,DWORD PTR [ebp-0x10]
                lea    ebx,[edx+eax*1]
                mov    eax,DWORD PTR [ebp-0x8]
                lea    edx,[eax+0x1]
                mov    eax,DWORD PTR [ebp+0x8]
                add    eax,edx
                movzx  eax,BYTE PTR [eax]
                movsx  edx,al
                mov    ecx,DWORD PTR [ebp-0x8]
                mov    eax,DWORD PTR [ebp+0x8]
                add    eax,ecx
                movzx  eax,BYTE PTR [eax]
                movsx  eax,al
                sub    edx,eax
                mov    eax,edx
                add    eax,ebx
                mov    DWORD PTR [ebp-0x10],eax
                add    DWORD PTR [ebp-0x8],0x1
        jump3: mov    eax,DWORD PTR [ebp-0xc]
                sub    eax,0x1
                cmp    DWORD PTR [ebp-0x8],eax
                jl     jump4
                mov    eax,DWORD PTR [ebp-0x10]
                add    esp,0x10
                pop    ebx
                pop    ebp
                ret
```
Now we can make a c wrapper, and call the assembly program inside it:
```c
#include <stdio.h>

int asm4(char*);

int main(int argc, char* argv[]) {
        printf("0x%x\n", asm4("picoCTF_f97bb"));
        return 0;
}
```
All thats left to do is compile the programs togther and get their output. We do these commands:
```sh
$ gcc -masm=intel -m32 -c mod_test.S -o mod_test.o
```
```sh
$ gcc -m32 -c asm4.c -o asm4.o
```
```sh
$ gcc -m32 mod_test.o asm4.o -o asm4
```
We run our executable and get our flag.

## Flag
0x265