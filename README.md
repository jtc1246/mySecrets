# MySecrets

## 1. Introduction

    This package includes three parts: hash, symmetrical encryption and jtc64
    (jtc64 is similar to base64, convert every 3 hex digits to 2 64-bit characters to reduce the length)
    
    The algorithm of hash and symmetrical encryption is very secure.
    
    Hash: If the original text changes slightly, the hash will be completely different.
          Known some part of the original text, you cannot get the other part reversely.

    Symmetrical encryption: Given ciphertext and some part of plaintext, you cannot get the key or other part of plaintext.
                            Given several ciphertext and corresponding plaintext, you cannot get the key.
    
## 2. Usage
    
### (1) Hash

    getHash(text:str)
    
    The return value format is a str with 64 hex digits.
    
### (2) Jtc64

    strToJtc64(text:str), jtc64ToStr(text:str)
    hexToJtc64(text:str), jtc64ToHex(text:str)
    
    The jtc64 output is a str containing 0~9, a~z, A~Z, @, # or $,%,&,= in the last.
    
    If the input/output is hex, note that it should be 0~9 and a~f in str, a~f should be in lower case, no "0x" at first.
    
### (3) Symmetrical encryption

    encrypt(text:str,key:str)
    decrypt(text:str,key:str)
    
    text: the str you want to encrypt (in encrypt) or the encrypted data (in decrypt) (the length must be larger than 0)
    key: any password you like
    
    The encrypted data is in jtc64 format.

## 3. Algorithms

### (1) Hash

    Step 1: Convert original text to hex
    Step 2: Supple the length of hex data to the multiple of 128 by adding '0' at first
    Step 3: Convert every 128 digits to 64 digits by adding a 128-digit hex number, then multipling with a 192-digit hex number
            and then taking the mid 64 digits, until it becomes 64 digits.
            If the length is not the multiple of 128, add 64 '0' at first.
            In the adding process, just add digit by digit, no carry.
            The 128-digit and 192-digit hex number is randomly generated when I write the program, it is a fixed number.
    Step 4: We finally get a 64-digit hex in step 3. Then create 20 groups, each group deal with this 64-digit hex in following process:
            ① add a 64-digit hex digit by digit
            ② disorder the digits of the hex text according to the pre-generated rule
            ③ multiple it with a 128-digit hex number, then take the mid 64 digits
            ④ disorder the digits of the hex text according to the rule in ②
            (Note: the 64-digit hex in ①, the 128-digit hex number in ③ and the rule in ② are randomly generated when I write the program, they are fixed)
    Step 5: Add the 20 64-digit hex numbers digit by digit, the result is the final hash.
    
    (You can see the pre-generated things in hash.py or others/standard.txt)
    
### (2) Jtc64

    Jtc64 is mainly composed of these characters: '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ@#'
    Each character stands for the number of its location. e.g. 0 stands for 0, b stands for 11, Z stands for 61
    Step 1: Convert original text to hex
    Step 2: Each time take 3 digits, calculate num, num = first digit * 256 + second digit * 16 + third digit
            num1 = num // 16; num2 = num % 16, convert num1 and num2 to corresponding jtc64 characters and put them together
            All 3 hex digits convert to 2 jtc64 characters, then combine the jtc64 characters in the original order
    Step 3: If there is no remaining hex digits, the convertion ends.
            If 1 hex digit remains, add it to the last of jtc64 str directly.
            If 2 hex digits remain, calculate num, num = first digit * 166 + second digit, num1 = num // 4; num2 = num % 4
            Add the jtc64 character corresponding to num1 to the last of jtc64 str, $ % & = each stands for 0,1,2,3 in num2, add it to the last of jtc64 str
