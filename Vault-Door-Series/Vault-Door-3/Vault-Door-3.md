# vault-door-3

## Description
This vault uses for-loops and byte arrays.

## Challenge Information
**Point Value:** 200  
**Category:** Reverse Engineering

## Hints
1. Make a table that contains each value of the loop variables and the corresponding buffer index that it writes to.

## Solution

```java
import java.util.*;

class VaultDoor3 {
    public static void main(String args[]) {
        VaultDoor3 vaultDoor = new VaultDoor3();
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

    // Our security monitoring team has noticed some intrusions on some of the
    // less secure doors. Dr. Evil has asked me specifically to build a stronger
    // vault door to protect his Doomsday plans. I just *know* this door will
    // keep all of those nosy agents out of our business. Mwa ha!
    //
    // -Minion #2671
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm12g94c_u_4_m7ra41");
    }
}
```
After making a table which looked like this:
```
First 8 index's of password: jU5t_a_s
Second 8 index's of password: na_3lpm1 (flipped) -> 1mpl3_an
Third 8 index's of password: 4|r|m|4|u|c|9|2 (splice this and next together)
Fourth 8 index's of password: 1|a|7|_|_|_|4|g (flipped) -> g|4|_|_|_7|a|1
Final password: jU5t_a_s1mpl3_an4gr4m_4_u_c79a21
```
Input the password and we get our flag.

## Flag
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}

