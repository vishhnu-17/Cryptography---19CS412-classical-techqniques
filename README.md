# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
```C
#include <stdio.h>
#include <stdlib.h>
 
void caesarEncrypt(char *text, int key) {
   for (int i = 0; text[i] != '\0'; i++) { char c = text[i];
    if (c >= 'A' && c <= 'Z') {
    text[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
    }
    else if (c >= 'a' && c <= 'z') {
        text[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
    }
    }
}

void caesarDecrypt(char *text, int key) {
caesarEncrypt(text, -key);
}

int main() {
char message[100];
int key;

printf("Enter the message to encrypt: ");
fgets(message, sizeof(message), stdin);  
printf("Enter the Caesar Cipher key (an integer): ");
scanf("%d", &key);
 
caesarEncrypt(message, key);
printf("Encrypted Message: %s", message);

 
caesarDecrypt(message, key);
printf("Decrypted Message: %s", message);
return 0;
}
```

## OUTPUT:

Simulating Caesar Cipher

<img width="1022" height="210" alt="Screenshot 2025-08-13 113716" src="https://github.com/user-attachments/assets/be70143b-e251-4366-90e9-78c24b27e0c6" />


Input : hello

Encrypted Message : mjqqt

Decrypted Message : hello

## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
``` C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 30

void toLowerCase(char plain[], int ps) {
    for (int i = 0; i < ps; i++) {
        if (plain[i] >= 'A' && plain[i] <= 'Z')
            plain[i] += 32;
    }
}

int removeSpaces(char* plain, int ps) {
    int i, count = 0;
    for (i = 0; i < ps; i++) {
        if (plain[i] != ' ')
            plain[count++] = plain[i];
    }
    plain[count] = '\0';
    return count;
}

void generateKeyTable(char key[], int ks, char keyT[5][5]) {
    int dicty[26] = {0};
    int i, j, k;

    for (i = 0; i < ks; i++) {
        if (key[i] != 'j')
            dicty[key[i] - 'a'] = 2;
    }
    dicty['j' - 'a'] = 1;

    i = j = 0;
    for (k = 0; k < ks; k++) {
        if (dicty[key[k] - 'a'] == 2) {
            dicty[key[k] - 'a']--;
            keyT[i][j] = key[k];
            j++;
            if (j == 5) { i++; j = 0; }
        }
    }

    for (k = 0; k < 26; k++) {
        if (dicty[k] == 0) {
            keyT[i][j] = (char)(k + 'a');
            j++;
            if (j == 5) { i++; j = 0; }
        }
    }
}

void search(char keyT[5][5], char a, char b, int arr[]) {
    if (a == 'j') a = 'i';
    if (b == 'j') b = 'i';

    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (keyT[i][j] == a) {
                arr[0] = i; arr[1] = j;
            }
            else if (keyT[i][j] == b) {
                arr[2] = i; arr[3] = j;
            }
        }
    }
}

int mod5(int a) { return (a % 5); }

int prepare(char str[], int ptrs) {
    if (ptrs % 2 != 0) {
        str[ptrs++] = 'z';
        str[ptrs] = '\0';
    }
    return ptrs;
}

void encrypt(char str[], char keyT[5][5], int ps) {
    int a[4];
    for (int i = 0; i < ps; i += 2) {
        search(keyT, str[i], str[i + 1], a);

        if (a[0] == a[2]) {  // same row
            str[i]     = keyT[a[0]][mod5(a[1] + 1)];
            str[i + 1] = keyT[a[0]][mod5(a[3] + 1)];
        }
        else if (a[1] == a[3]) {  // same column
            str[i]     = keyT[mod5(a[0] + 1)][a[1]];
            str[i + 1] = keyT[mod5(a[2] + 1)][a[1]];
        }
        else {  // rectangle swap
            str[i]     = keyT[a[0]][a[3]];
            str[i + 1] = keyT[a[2]][a[1]];
        }
    }
}

void encryptByPlayfairCipher(char str[], char key[]) {
    int ps, ks;
    char keyT[5][5];

    // Key
    ks = strlen(key);
    ks = removeSpaces(key, ks);
    toLowerCase(key, ks);

    // Plaintext
    ps = strlen(str);
    toLowerCase(str, ps);
    ps = removeSpaces(str, ps);
    ps = prepare(str, ps);

    generateKeyTable(key, ks, keyT);
    encrypt(str, keyT, ps);
}

int main() {
    char str[SIZE], key[SIZE];

    strcpy(key, "Monarchy");
    printf("Key text: %s\n", key);

    strcpy(str, "instruments");
    printf("Plain text: %s\n", str);

    encryptByPlayfairCipher(str, key);
    printf("Cipher text: %s\n", str);

    return 0;
}

```

## OUTPUT:
<img width="1076" height="224" alt="Screenshot 2025-08-18 132811" src="https://github.com/user-attachments/assets/6ec105cc-2d88-4ac9-a6b6-76f5fcd8ce6a" />

Output:
Key text: Monarchy

Plain text: instruments 

Cipher text: gatlmzclrqtx

## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:
``` C
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Key matrix
int keymat[3][3] = {
    { 1, 2, 1 },
    { 2, 3, 2 },
    { 2, 2, 1 }
};

// Inverse key matrix (precomputed)
int invkeymat[3][3] = {
    { -1,  0,  1 },
    {  2, -1,  0 },
    { -2,  2, -1 }
};

char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// Encode 3 characters
void encode(char a, char b, char c, char ret[]) {
    int posa = a - 'A';
    int posb = b - 'A';
    int posc = c - 'A';

    int x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    int y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    int z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];

    ret[0] = key[x % 26];
    ret[1] = key[y % 26];
    ret[2] = key[z % 26];
    ret[3] = '\0';
}

// Decode 3 characters
void decode(char a, char b, char c, char ret[]) {
    int posa = a - 'A';
    int posb = b - 'A';
    int posc = c - 'A';

    int x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    int y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    int z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];

    ret[0] = key[(x % 26 < 0) ? (26 + x % 26) : (x % 26)];
    ret[1] = key[(y % 26 < 0) ? (26 + y % 26) : (y % 26)];
    ret[2] = key[(z % 26 < 0) ? (26 + z % 26) : (z % 26)];
    ret[3] = '\0';
}

int main() {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    char temp[4];
    int n;

    strcpy(msg, "SecurityLaboratory");
    printf("Simulation of Hill Cipher\n");
    printf("Input message : %s\n", msg);

    // Convert to uppercase
    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    // Padding (mod 3)
    n = strlen(msg) % 3;
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }

    printf("Padded message : %s\n", msg);

    // Encoding
    for (int i = 0; i < strlen(msg); i += 3) {
        encode(msg[i], msg[i + 1], msg[i + 2], temp);
        strcat(enc, temp);
    }

    printf("Encoded message : %s\n", enc);

    // Decoding
    for (int i = 0; i < strlen(enc); i += 3) {
        decode(enc[i], enc[i + 1], enc[i + 2], temp);
        strcat(dec, temp);
    }

    printf("Decoded message : %s\n", dec);

    return 0;
}

```


## OUTPUT:
Simulating Hill Cipher
<img width="1140" height="214" alt="Screenshot 2025-08-18 133650" src="https://github.com/user-attachments/assets/fc729f03-a057-4a29-add8-d4ffb2da929f" />


Input Message : SecurityLaboratory

Padded Message : SECURITYLABORATORY

Encrypted Message : EACSDKLCAEFQDUKSXU 

Decrypted Message : SECURITYLABORATORY
## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
``` C
#include <stdio.h>
#include <string.h>

void vigenereEncrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];

        if (c >= 'A' && c <= 'Z') {
            // Encrypt uppercase letters
            text[i] = ((c - 'A' + (key[i % keyLen] - 'A')) % 26) + 'A';
        }
        else if (c >= 'a' && c <= 'z') {
            // Encrypt lowercase letters
            text[i] = ((c - 'a' + (key[i % keyLen] - 'A')) % 26) + 'a';
        }
    }
}

void vigenereDecrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];

        if (c >= 'A' && c <= 'Z') {
            // Decrypt uppercase letters
            text[i] = ((c - 'A' - (key[i % keyLen] - 'A') + 26) % 26) + 'A';
        }
        else if (c >= 'a' && c <= 'z') {
            // Decrypt lowercase letters
            text[i] = ((c - 'a' - (key[i % keyLen] - 'A') + 26) % 26) + 'a';
        }
    }
}

int main() {
    const char *key = "KEY";   // Replace with your desired key
    char message[] = "This is a secret message.";  // Replace with your message

    printf("Original Message: %s\n", message);

    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);

    // Decrypt the message back to the original
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);

    return 0;
}

```
## OUTPUT:

Simulating Vigenere Cipher
<img width="996" height="224" alt="Screenshot 2025-08-18 134200" src="https://github.com/user-attachments/assets/de7dfc7c-92da-45a9-a8fe-6be05fed3ace" />


Input Message : This is a secret message.


Encrypted Message : Dlgc gc y wcmvcd kowqkkc

Decrypted Message : This is a secret message.
## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:
``` C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main() {
    int i, j, len, rails;
    char str[1000];

    printf("Enter a Secret Message:\n");
    fgets(str, sizeof(str), stdin);

    str[strcspn(str, "\n")] = '\0';
    len = strlen(str);

    printf("Enter number of rails:\n");
    scanf("%d", &rails);

    char code[rails][len];

    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            code[i][j] = '\n';
        }
    }

    // Fill the rail fence zig-zag
    int row = 0, down = 1;
    for (j = 0; j < len; j++) {
        code[row][j] = str[j];

        if (row == 0) {
            down = 1;  // Go down
        } else if (row == rails - 1) {
            down = 0;  // Go up
        }

        row += (down ? 1 : -1);
    }

    // Print encrypted text
    printf("Encrypted Message: ");
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            if (code[i][j] != '\n') {
                printf("%c", code[i][j]);
            }
        }
    }
    printf("\n");

    return 0;
}
```

## OUTPUT:
<img width="1105" height="227" alt="Screenshot 2025-08-18 135114" src="https://github.com/user-attachments/assets/89da593b-98c7-4098-a10c-41d94664b916" />

Enter a Secret Message: Weardiscovered

Enter number of rails: 2

Encrypted message: Wadsoeeericvrd
## RESULT:
The program is executed successfully
