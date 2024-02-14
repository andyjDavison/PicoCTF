# asm2

## Description
What does asm2(0xb,0x2e) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format.

## Challenge Information
**Point Value:** 250  
**Category:** Reverse Engineering

## Hints
1. assembly [conditions](https://www.tutorialspoint.com/assembly_programming/assembly_conditions.htm)

## Solution
We are given an assembly program:
```s
asm2:
        <+0>:   push   ebp
        <+1>:   mov    ebp,esp
        <+3>:   sub    esp,0x10
        <+6>:   mov    eax,DWORD PTR [ebp+0xc] # store the second parameter (0x2e) in eax
        <+9>:   mov    DWORD PTR [ebp-0x4],eax # then move that parameter in the beginning of stack
        <+12>:  mov    eax,DWORD PTR [ebp+0x8] # get the original first parameter (0xb) and store in eax
        <+15>:  mov    DWORD PTR [ebp-0x8],eax # move that value to the begninning of stack again
        <+18>:  jmp    0x509 <asm2+28>
        <+20>:  add    DWORD PTR [ebp-0x4],0x1 # 0x2e + 0x1 = 0x2f
        <+24>:  sub    DWORD PTR [ebp-0x8],0xffffff80 # 0xb - 0xffffff80 = 0xffffffff0000008b
        <+28>:  cmp    DWORD PTR [ebp-0x8],0x63f3 # compare 0xb and 0x63f3
        <+35>:  jle    0x501 <asm2+20>
        <+37>:  mov    eax,DWORD PTR [ebp-0x4] # move 0x2f into eax
        <+40>:  leave  
        <+41>:  ret 
```
I try to figure out what is going on in the registers, but hex math isn't my thing so let's just write a c program to figure it out. First we need to modify our assembly program:
```s
.intel_syntax noprefix
        .global asm2

asm2:
        push   ebp
        mov    ebp,esp
        sub    esp,0x10
        mov    eax,DWORD PTR [ebp+0xc]
        mov    DWORD PTR [ebp-0x4],eax
        mov    eax,DWORD PTR [ebp+0x8]
        mov    DWORD PTR [ebp-0x8],eax
        jmp    comp
  added:add    DWORD PTR [ebp-0x4],0x1
        sub    DWORD PTR [ebp-0x8],0xffffff80
  comp: cmp    DWORD PTR [ebp-0x8],0x63f3
        jle    added
        mov    eax,DWORD PTR [ebp-0x4]
        leave  
        ret 
```
Now that we have a functioning asm program, let's write our c wrapper:
```c
#include <stdio.h>

int asm2(int, int);

int main(int argc, char* argv[]) {
        printf("0x%x\n", asm2(0xb,0x2e));
        return 0;
}
```
Now we just need to compile these two programs:
```sh
$ gcc -masm=intel -m32 -c mod_test.S -o mod_test.o
```
```sh
$ gcc -m32 -c asm2.c -o asm2.o
```
```sh
$ gcc -m32 mod_test.o asm2.o -o asm2
```
Then we just run our executable and we get our flag.
## Flag
0xf6