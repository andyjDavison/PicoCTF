# PW Crack 2

## Description
Can you crack the password to get the flag?

## Challenge Information
**Point Value:** 100  
**Category:** General Skills

## Hints
1. Does that encoding look familiar?
2. The str_xor function does not need to be reverse engineered for this challenge.

## Solution
We are given a python program that once again needs a password to get the flag. Lets look at the code:
```py
### THIS FUNCTION WILL NOT HELP YOU FIND THE FLAG --LT ########################
def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
###############################################################################

flag_enc = open('level2.flag.txt.enc', 'rb').read()



def level_2_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65) ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")



level_2_pw_check()
```
We can once again find the password in the code, but this time its a string of hex characters:
```py
user_pw == chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65)
```
We can take these hex characters and convert them to ascii to get our password, ```39ce```

## Flag
picoCTF{tr45h_51ng1ng_502ec42e}