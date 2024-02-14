# Glitch Cat

## Description
Our flag printing service has started glitching!

## Challenge Information
**Point Value:** 100  
**Category:** General Skills

## Hints
1. ASCII is one of the most common encodings used in programming
2. We know that the glitch output is valid Python, somehow!
3. Press Ctrl and c on your keyboard to close your connection and return to the command prompt.

## Solution
We connect to the server using ```$ nc saturn.picoctf.net 52682``` and we get out this ```picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x64) + chr(0x61) + chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + '}```. We know these are ASCII values, and they will work in a python script. The script: 
```python
print("picoCTF{gl17ch_m3_n07_" + chr(0x62) + chr(0x64) + chr(0x61) + chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + "}")
```
We run the script and it prints the flag.

## Flag
picoCTF{gl17ch_m3_n07_bda68f75}