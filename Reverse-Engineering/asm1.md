# asm1

## Description
What does asm1(0x8be) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format.

## Challenge Information
**Point Value:** 200  
**Category:** Reverse Engineering

## Hints
1. assembly [conditions](https://www.tutorialspoint.com/assembly_programming/assembly_conditions.htm)

## Solution
We are given an assembly program, and asked to find what is in eax register, when we run asm1(0x8be).
```s
asm1:
        <+0>:   push   ebp
        <+1>:   mov    ebp,esp
        <+3>:   cmp    DWORD PTR [ebp+0x8],0x71c
        <+10>:  jg     0x512 <asm1+37>
        <+12>:  cmp    DWORD PTR [ebp+0x8],0x6cf
        <+19>:  jne    0x50a <asm1+29>
        <+21>:  mov    eax,DWORD PTR [ebp+0x8]
        <+24>:  add    eax,0x3
        <+27>:  jmp    0x529 <asm1+60>
        <+29>:  mov    eax,DWORD PTR [ebp+0x8]
        <+32>:  sub    eax,0x3
        <+35>:  jmp    0x529 <asm1+60>
        <+37>:  cmp    DWORD PTR [ebp+0x8],0x8be
        <+44>:  jne    0x523 <asm1+54>
        <+46>:  mov    eax,DWORD PTR [ebp+0x8]
        <+49>:  sub    eax,0x3
        <+52>:  jmp    0x529 <asm1+60>
        <+54>:  mov    eax,DWORD PTR [ebp+0x8]
        <+57>:  add    eax,0x3
        <+60>:  pop    ebp
        <+61>:  ret
```
We know that 0x8be is pushed into the stack, and is in ebp. Our first comparison is:
```s
<+3>:   cmp    DWORD PTR [ebp+0x8],0x71c
<+10>:  jg     0x512 <asm1+37>
```
We are comparing 0x71c and the first value in the stack which is 0x8be. ```jg``` means to jump if greater, and we know that 0x8be is greater, so we jump to the comparison on line 37.
```s
<+37>:  cmp    DWORD PTR [ebp+0x8],0x8be
<+44>:  jne    0x523 <asm1+54>
```
Here we are comparing 0x8be to the first value in the stack, which is 0x8be. ```jne``` means to jump if not equal, and we know these are the same value, so we just continue to the next lines:
```s
<+46>:  mov    eax,DWORD PTR [ebp+0x8]
<+49>:  sub    eax,0x3
```
Here we move the first value in the stack to the eax register, and then subtract 0x3 from it. ```0x8be - 0x3 = 0x8bb```

## Flag
0x8bb