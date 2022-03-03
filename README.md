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
