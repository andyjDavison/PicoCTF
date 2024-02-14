# asm3

## Description
What does asm3(0xba6c5a02,0xd101e3dd,0xbb86a173) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format.

## Challenge Information
**Point Value:** 300  
**Category:** Reverse Engineering

## Hints
1. more(?) [registers](https://wiki.skullsecurity.org/index.php?title=Registers)

## Solution
We are given an assembly program:
```s
asm3:
        <+0>:   push   ebp
        <+1>:   mov    ebp,esp
        <+3>:   xor    eax,eax
        <+5>:   mov    ah,BYTE PTR [ebp+0xb] # ah is bits 15..8 of ah -> 0xba
        <+8>:   shl    ax,0x10 # sets ax to 0
        <+12>:  sub    al,BYTE PTR [ebp+0xd] # al is bits 7..0 of ax -> al = 0x0 - 0xe3 = 00000000 - 11100011 ->
        <+15>:  add    ah,BYTE PTR [ebp+0xc] # ah is bits 15..8 of ax -> ah = 0xba + 0xdd = 0x197
        <+18>:  xor    ax,WORD PTR [ebp+0x12] # ax -> 0x197
        <+22>:  nop
        <+23>:  pop    ebp
        <+24>:  ret 
```
```
               0  1  2  3   byte offsets
     + 0x10 | 73 a1 86 bb |
     + 0x0c | dd e3 01 d1 |
     + 0x08 | 02 5a 6c ba |
     + 0x04 | return      |
 ebp + 0x00 | prev. ebp   |
```
I tried my best to create the stack, but honestly the program does too much math with hex values and I would rather just write a c program that runs the asm program. We need to modify our original asm program: 
```s
.intel_syntax noprefix
 .global asm3

 asm3:
     push   ebp
     mov    ebp,esp
     xor    eax,eax
     mov    ah,BYTE PTR [ebp+0xb]
     shl    ax,0x10
     sub    al,BYTE PTR [ebp+0xd]
     add    ah,BYTE PTR [ebp+0xc]
     xor    ax,WORD PTR [ebp+0x12]
     nop
     pop    ebp
     ret
```
We can now create a c wrapper, that runs our asm3 function and outputs our flag to the screen:
```c
#include <stdio.h>

int asm3(int,int,int);

int main(int argc, char* argv[]) {
        printf("0x%x\n", asm3(0xba6c5a02,0xd101e3dd,0xbb86a173));
        return 0;
}
```
We can then run this with the following terminal commands:
```sh
$ gcc -masm=intel -m32 -c mod_test.S -o mod_test.o
```
```sh
$ gcc -m32 -c asm3.c -o asm3.o
```
```sh
$ gcc -m32 mod_test.o asm3.o -o asm3
```
and it prints out the flag.

## Flag
0x669b