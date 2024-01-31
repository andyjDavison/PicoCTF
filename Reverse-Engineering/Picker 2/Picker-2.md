# Picker II

## Description
Can you figure out how this program works to get the flag?  
Connect to the program with netcat: ```$ nc saturn.picoctf.net 61459```

## Challenge Information
**Point Value:** 100  
**Category:** Low Level Binary

## Hints
1. Can you do what win does with your input to the program?

## Solution
We are given a python file that supposedly outputs a random number. If we look through the file, we see that it evaluates the passed in function and runs it: 
```py
while(True):
  try:
    user_input = input('==> ')
    if( filter(user_input) ):
      eval(user_input + '()')
    else:
      print('Illegal input')
  except Exception as e:
    print(e)
```
We notice that the win function prints out our flag,
```py
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
so we try typing win into the prompt, but we get out: ```Illegal input```. We notice that there is a filter function that the user_input is passed too to check the function name, so we cant just input win.
```py
def filter(user_input):
  if 'win' in user_input:
    return False
  return True
```
Instead we can try running the functions that are inside of the win function to see if we get the flag. We put ```print(open('flag.txt', 'r').read())``` into the prompt and we get out ```picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_b924e8e5}```, and we got the flag.

## Flag
picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_b924e8e5}