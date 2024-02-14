# HashingJobApp

## Description
If you want to hash with the best, beat this test!

## Challenge Information
**Point Value:** 100  
**Category:** General Skills

## Hints
1. You can use a commandline tool or web app to hash text
2. Press Ctrl and c on your keyboard to close your connection and return to the command prompt.

## Solution
We load into the remote program using ```$ nc saturn.picoctf.net ```, and we get this as our solution.
```
Please md5 hash the text between quotes, excluding the quotes: 'UV rays'
Answer: 
fa572056bc7677e6104801f2773fcc76
fa572056bc7677e6104801f2773fcc76
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'a car crash'
Answer: 
55067b2a1b8b8110a7411ba64e6f6168
55067b2a1b8b8110a7411ba64e6f6168
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'crawl space'
Answer: 
a320efce66b14e02f4569a08959ad2e8
a320efce66b14e02f4569a08959ad2e8
Correct.
picoCTF{4ppl1c4710n_r3c31v3d_bf2ceb02}
```
All I did here, was take the text between quotes and put them in an md5 hasher online.

## Flag
picoCTF{4ppl1c4710n_r3c31v3d_bf2ceb02}