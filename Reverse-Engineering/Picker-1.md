# Picker I

## Description
This service can provide you with a random number, but can it do anything else?  
Connect to the program with netcat:
```$ nc saturn.picoctf.net 61758```

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Binary

## Hints
1. Can you point the program to a function that does something useful for you?

## Solution
We run the instance of the program, and we are prompted to enter getRandomNumber without parenthesis, to get a random number. If we take a look at the python file,
```python
while(True):
  try:
    print('Try entering "getRandomNumber" without the double quotes...')
    user_input = input('==> ')
    eval(user_input + '()')
  except Exception as e:
    print(e)
``` 
we see that the program will evaluate whatever function we pass in and run it. We can see the function win() will provide us with the flag if we can get it to run, so we pass win to the program.
```python
def win():
  # This line will not work locally unless you create your own 'flag.txt' in
  #   the same directory as this script
  flag = open('flag.txt', 'r').read()
  #flag = flag[:-1]
  flag = flag.strip()
  str_flag = ''
  for c in flag:
    str_flag += str(hex(ord(c))) + ' '
  print(str_flag)
```
So we run the program again and when prompted for input we just type win. We get out some hex, in this format:
```
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x5f 0x64 0x31 0x34 0x6d 0x30 0x6e 0x64 0x5f 0x31 0x6e 0x5f 0x37 0x68 0x33 0x5f 0x72 0x30 0x75 0x67 0x68 0x5f 0x62 0x35 0x32 0x33 0x62 0x32 0x61 0x31 0x7d
```
Find a hex to ASCII converter, and we get the flag.

## Flag
picoCTF{4_d14m0nd_1n_7h3_r0ugh_b523b2a1}