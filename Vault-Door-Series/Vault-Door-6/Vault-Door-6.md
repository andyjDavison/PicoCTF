# vault-door-6

## Description
This vault uses an XOR encryption scheme.

## Challenge Information
**Point Value:** 350  
**Category:** Reverse Engineering

## Hints
1. If X ^ Y = Z, then Z ^ Y = X. Write a program that decrypts the flag based on this fact.

## Solution
We are given a Java program that needs a password:
```java
import java.util.*;

class VaultDoor6 {
    public static void main(String args[]) {
        VaultDoor6 vaultDoor = new VaultDoor6();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
        String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
        if (vaultDoor.checkPassword(input)) {
            System.out.println("Access granted.");
        } else {
            System.out.println("Access denied!");
        }
    }

    // Dr. Evil gave me a book called Applied Cryptography by Bruce Schneier,
    // and I learned this really cool encryption system. This will be the
    // strongest vault door in Dr. Evil's entire evil volcano compound for sure!
    // Well, I didn't exactly read the *whole* book, but I'm sure there's
    // nothing important in the last 750 pages.
    //
    // -Minion #3091
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
            0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
            0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
            0xa , 0x66, 0x36, 0x30, 0x67, 0x6c, 0x64, 0x6c,
        };
        for (int i=0; i<32; i++) {
            if (((passBytes[i] ^ 0x55) - myBytes[i]) != 0) {
                return false;
            }
        }
        return true;
    }
}
```
When we look in the method checkPassword() we see that it does an XOR operation on the password and checks if it is equal to the values in the myBytes array. We can use this array to write our own program to output the string, which just reverses the XOR and outputs the character after each operation:
```java
import java.util.*;

class decode {
        public static void main(String args[]) {
                byte[] myBytes = {
                0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
                0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
                0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
                0xa , 0x66, 0x36, 0x30, 0x67, 0x6c, 0x64, 0x6c,
                };
                for(int i=0;i<32;i++) {
                        System.out.println((char)(myBytes[i] ^ 0x55));
                }
        }
}
```
We run our program and get the flag.

## Flag
picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_3ce2919}