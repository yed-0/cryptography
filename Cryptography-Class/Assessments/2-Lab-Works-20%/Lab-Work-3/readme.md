# Lab 3: Hands-on Exploration of Cryptographic Tools: Hashing, Encryption, and Digital Signature

## lab objective 💪

in this lab we will be introduced to openSSL which is a cryptography toolkit.We will be performing essential cryptographic operations such as :

- Symmetric encryption(AES)
- Asymmetric encryption (RSA)
- Hashing (SHA-256)
- Digital signatures (RSA with SHA-256)

 after you follow me step by step on this labwork,you will be able to :
- Encrypt and decrypt files using symmetric and asymmetric encryption.
- Generate and verify hashes for data integrity.
- Create and verify digital signatures.

--- 

##  **Tools Used**:

1. OpenSSL
2. [Wormhole](https://wormhole.app/) (end-to-end encryption private file sharing)\



 **NOW LETS START** 💪



--- 

## **Task 1: Symmetric Encryption using AES**

**Objective: Encrypt and decrypt a file using AES-256 in CBC modde.I will encrypt and send it securely to adam so he can decrypt it safely.**

**STEPS:**

### 1. create a .txt file fill with any text that you want to encrypt
 
**command:**
```bash
 echo "your.text">filename.txt
```

*replace your.text and filename on your own. Refer the screenshot if you are confuse on how to replace the command. Please take not that This apply to entire of  labwork proccess.If you still dont get it then it is a skill isues ><*

**example:**

```bash
┌──(syed㉿NWS23010037)-[~]
└─$ echo "kelisa putih nampak rare" > "syed.txt"

```



###  2. generate key and iv

**command:**
```bash
openssl rand -hex 32 > filename
openssl rand -hex 16 > filename
```

> *openssl rand -hex 32 generates a 256-bit (32 bytes) key in hexadecimal.*

> *openssl rand -hex 16 generates a 128-bit IV (Initialization Vector).*


**example:**

```bash
┌──(syed㉿NWS23010037)-[~]
└─$ openssl rand -hex 32 > key

┌──(syed㉿NWS23010037)-[~]
└─$ openssl rand -hex 16 > iv

┌──(syed㉿NWS23010037)-[~]
└─$ cat key iv
595369a67de06fa9619e2cab404180e958e07dc258423b6509b11dfd4c4aa8f3
bac6130b2c33397a5afbf851cfd0acb9

```



###  3. encrypt the file using AES-256-CBC.

**command:**
```bash
openssl enc -e -aes-256-cbc -K $(cat key) -iv $(cat iv) -in filename -out filename.enc

```
> *-aes-256-cbc: specifies AES with 256-bit key in CBC mode.*

> *-K and -iv: pass the key and IV in hexadecimal format.*

> *-e: encryption mode.*



**example:**


```bash
┌──(syed㉿NWS23010037)-[~]
└─$ openssl enc -e -aes-256-cbc -K $(cat key) -iv $(cat iv) -in syed.txt -out syed.txt.enc

┌──(syed㉿NWS23010037)-[~]
└─$ cat syed.txt.enc 
vlEt�t�����'(��kB�5����4�z��4

```



###  4. Decrypt the file

**command:**
```bash
openssl enc -e -aes-256-cbc -K $(cat filename) -iv $(cat filename) -in filename -out filename

```

**example:**
```bash
┌──(adamriezqie㉿NWS23010043)-[~/Downloads]
└─$ openssl enc -d -aes-256-cbc -K $(cat key) -iv $(cat iv) -in syed.txt.enc -out syed.decrypt
                                                                                                         
┌──(adamriezqie㉿NWS23010043)-[~/Downloads]
└─$ cat syed.decrypt
kelisa putih nampak rare

```



### 5.  Verify the decrypted content matches the original

**command:**
```bash
ll filename filename.decrypt
diff filename filename.decrypt
```

**example:**


```bash
┌──(syed㉿NWS23010037)-[~]
└─$ ll syed.txt syed.decrypt
-rw-rw-r-- 1 syed syed 25 May  2 00:53 syed.decrypt
-rw-rw-r-- 1 syed syed 25 May  2 00:05 syed.txt                                                                                                                        
┌──(syed㉿NWS23010037)-[~]                                       
└─$ diff syed.txt syed.decrypt 
                                                                                                                                                                                                    
┌──(syed㉿NWS23010037)-[~]                                      
└─$                                                                                                                                                   
```




## **Task 2: Asymmetric Encryption using RSA**

**Objective: Generate an RSA key pair, encrypt a message with the public key, and decrypt it with the private key.**


**STEPS:**

### 1. Generate an RSA private key (2048-bit)

**command:**
```bash
openssl genpkey -algorithm RSA -out filename -pkeyopt rsa_keygen_bits:2048
```

**example:**

```bash
┌──(syed㉿NWS23010037)-[~]
└─$ openssl genpkey -algorithm RSA -out private.key  -pkeyopt rsa_keygen_bits:2048
..+..+....+.........+............+++++++++++++++++++++++++++++++++++++++*........+.......+...........+.......+...+..+.......+++++++++++++++++++++++++++++++++++++++*..+..........+.........+...........+.......+..+.........+.......+...+...............+..+............+.+......+....................+........................+.+...+..+...+...+...+.+...+...+...............+..+.....................+.+......+..+.+.........+....................+.+....................+...............+.......+..++++++                                                                                        
.+.+++++++++++++++++++++++++++++++++++++++*..........+..+.+..+...+.......+++++++++++++++++++++++++++++++++++++++*.......................+..+............+..................+.++++++          
┌──(syed㉿NWS23010037)-[~]
└─$ cat private.key                                                                                                                                
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCmoutBA9yAzo/L
YMyC3DzUrCHjfI8e4W48blXtn1MJDPdk3/ybWA6egMv7wF8zZubmlkoFB9P19XsW
dnegy2QyVChzW5+ny4FFKUXYVaMYk/pWcnVV1UB2P+IYXB6gJ/NX+jIoCOeTsXOY
sdWMiozIxvVpCn1hQQb68x2W8tqOg+Bb4sKpIgL0tXDkqWiRpyJnJXosiy836rW5
74kNyyMc4iiRms71lj8urEyN9MyQ364xko7DJFfCpBJ1ABh2yvDbIoF54q+CDpL8
Zg0DA4T83Dp2MLCAYWKdWBCeXjrlOrKdP4YY0EPsLRvjpL/V10ShG6iamGS2jj4L
FY0ShcPrAgMBAAECggEAIM3CvTZK4sqHMTTZnctIaF+IflWHgT9Eqb3YyaILQSVd
0GxtuJERs69MOL+qnh5cRzl/0dS0K9/K/uWMcEmJ7IR3wBnh6LDdvmU9HDTjmhOL
fRgkNCT6fyzdUAlWeBXVSFT3Kb1zBTayOHWNwhCGXYIZTrfjbdFU0/MnOI+Pae2H
lW3NSSo1FUPbdCbyZSA0NOo/qQSyVzOnB/V6ZK4klKFVaEKCv4g1XanaG+m4Z9HX
Pmq1DyuVe3scrBGThxfdalLc1NShCQFZ0Uv8Vn7nt8bST3og1rDUvQyMRX35V9P3
R01sjEDxhkbNZri3Hhb2uFMd8TNWRb+2uTNpSR4TuQKBgQDgRM121VP+BnG1q9MU
Yf/FxpfMhKrMjzWbsNfBe+m1jSZf59IdZUZyjsQXWQvtOOzwr1QPgk/PBPi/MfOg
Kko1wdlS4/tcL+bTwAxZSfmuS94O9NZVug5c5KC8VoYkLTT1kBxabIuXM1Wl3PF9
Cd8ln6z0SkvdokMYuHpFNDfNMwKBgQC+NqAXX7w8Ph9dhlrB/nOx44Dg2rxu7H11
Uca+vyQVjcpoOd9+Huv2iOzJPM93r0pp7ShGFsfhpQXVn+M3EpqntXEpbrwpEsAV
JD7Xo2zKKsQ1M9GphhwwRsiqU9gSeffi3bgm93ja+IzHmU4s+87pZRr2isofTOy8
KIcNKc7+aQKBgQCeHiQ2ikWDrgtxqMlTb5yjVlckLM03GCkVseMMWU73X6MUrx8m
9oZqsllylKeiAyaasyKzJI8cSEcQlilMXUShDiEpHe/UmTlHRfnu4gcVagwPw3Ny
UZFa0R66as58pxrVn1s+LZVlbx4NqO7h15nqrt+EBPXkN0jTHw2lySEmnQKBgBsG
5/hqQfTNXueLD5pA7W95Q/avHdclAy6IUBUKU7Y8T2Y/0uG4ww5kpkBxw4jd/1TN
vcs3sW7+Y7XW4hIRMqhhv5KDiXjMkT5vx/4b2nlfWkG0+zV7OMYjSJ/rCeiWLAKw
PlpLD4ENj/NMdS8vGFdRZmNhlRadd4XEvGvCZQcBAoGAFHtrYMIaGgRMz7b8tKRJ
BbABKXcM3JkpUmHb3j0WlMX1wUpF21V1l1XUtnAW/Q0USwvMClgGnnkEvth/lkA7
9ANRgsB5Fsg/EjUIXVZZ8Rs3rNSeTAsny7vyHpGyn3zsBizUsVVq+NOEOmSfEusr
yiCkQ3SgWYY2hAfUJ0oPr10=
-----END PRIVATE KEY-----


```

### 2. Extract the public key

**command:**
```bash
 openssl rsa -in filename -pubout -out filename

```

**example:**

```bash
┌──(syed㉿NWS23010037)-[~]                                                                                          
└─$ openssl rsa -in private.key -pubout -out public.key                                                             
writing RSA key

┌──(syed㉿NWS23010037)-[~]
└─$ cat public.key                                                                                                  
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEApqLrQQPcgM6Py2DMgtw8
1Kwh43yPHuFuPG5V7Z9TCQz3ZN/8m1gOnoDL+8BfM2bm5pZKBQfT9fV7FnZ3oMtk
MlQoc1ufp8uBRSlF2FWjGJP6VnJ1VdVAdj/iGFweoCfzV/oyKAjnk7FzmLHVjIqM
yMb1aQp9YUEG+vMdlvLajoPgW+LCqSIC9LVw5KlokaciZyV6LIsvN+q1ue+JDcsj
HOIokZrO9ZY/LqxMjfTMkN+uMZKOwyRXwqQSdQAYdsrw2yKBeeKvgg6S/GYNAwOE
/Nw6djCwgGFinVgQnl465TqynT+GGNBD7C0b46S/1ddEoRuomphkto4+CxWNEoXD
6wIDAQAB
-----END PUBLIC KEY-----


```



### 3. Encrypt a message using the public key

**command:**
```bash
 openssl pkeyutl -encrypt -inkey filename -pubin -in filename -out filename       
```

**example:**

```bash
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-2]
└─$ openssl pkeyutl -encrypt -inkey public.key -pubin -in secret.txt -out secret.enc

┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-2]
└─$ cat secret.enc  
�HV▒$�$
       0���f�7,b�����6�U��g����wz4��������▒���[��{r��"�.�~���A��㛌�ᠮ{�k2J��o�E��=1N�{�x'��+[UN�����*���&I���#;3�6����|Y���J6�\$}E�~N���{4C5NaJsT�s�����-SF#���Jj��&�ZF��n;�5}����ܗ�Ma&o+��&�_H���.�
                         I��nڇ�#*��ī�!�S����r�I���<   
```


### 4. Decrypt using the private key

**command:**
```bash
   openssl pkeyutl -decrypt -inkey filename -in filename -out filename
```

**example:**

```bash
┌──(syed㉿NWS23010037)-[~]
└─$ openssl pkeyutl -decrypt -inkey private.key -in secret.enc -out secret.decrypt

┌──(syed㉿NWS23010037)-[~]
└─$ cat secret.decrypt                                                                                              
hi syed, long time no see... here secret message for you :143

```


### 5. Verify the decrypted message matches the original.

**command:**
```bash
   diff filename.txt filname.txt.decrypt
```
*if there is nothing came out then it matches but if it is not match then the different will be shown.*


**example:**

   ```bash                                                                                  
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-2]
└─$ cat secret.decrypt secret.txt
hi syed, long time no see... here secret message for you :143
hi syed, long time no see... here secret message for you :143
                                                                                     
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-2]
└─$ ll secret.decrypt secret.txt
-rw-rw-r-- 1 adamriezqie adamriezqie 62 May  2 14:22 secret.decrypt
-rw-rw-r-- 1 adamriezqie adamriezqie 62 May  2 13:30 secret.txt
                                                                                     
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-2]
└─$ diff secret.decrypt secret.txt
                                                                                     
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-2]
└─$ 
```

## **Task 3:  Hashing and Message Integrity using SHA-256**

**Objective: Generate a hash of a file and verify its integrity.**


**STEPS:**

### 1. Create a sample file

**command:**
```bash
echo "anything" > filename
```

**example:**

```bash
┌──(syed㉿NWS23010037)-[~]
└─$ echo "27/6/2025 12.A.M lobby" > info.txt

┌──(syed㉿NWS23010037)-[~]
└─$ cat info.txt 
27/6/2025 12.A.M lobby

```

### 2. Generate SHA-256 hash

**command:**
```bash
openssl dgst -sha256 filename
```

**example:**

```bash
┌──(syed㉿NWS23010037)-[~]
└─$ openssl dgst -sha256 info.txt 
SHA2-256(info.txt)= 6cca854084069003bec800a1dd1b07cf54f8da311f3db783afa095ab03693d45
```


### 3. Modify the file slightly 

**command:**

```bash
echo "anything" > filename
```

**example:**

```bash
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-3]
└─$ cat info.txt                 
27/6/2025 12.A.M lobby
                                                                                                                              
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-3]
└─$ echo "\n\n\nHope syed did not see this comming" >> info.txt                      
                                                                                                                              
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-3]
└─$ cat info.txt 
27/6/2025 12.A.M lobby

Hope syed did not see this comming   
```

### 4. regenerate the hash and check it

**command:**

```bash
openssl dgst -sha256 filename
```

**example:**

```bash
┌──(syed㉿NWS23010037)-[~]                                                                                          
└─$ openssl dgst -sha256 info1.txt 
SHA2-256(info1.txt)= 8c012278b413e1d1c18536553d8784f7436b0996b9b6b1fb95f50acae8d42556                               
                                                                                                                    
┌──(syed㉿NWS23010037)-[~]                                                                                          
└─$ openssl dgst -sha256 info.txt 
SHA2-256(info.txt)= 6cca854084069003bec800a1dd1b07cf54f8da311f3db783afa095ab03693d45

```

## **Task 4:  Digital Signatures using RSA & SHA-256**

**Objective: Sign a file and verify the signature.**


**STEPS:**

### 1.create and  Sign the file using Private Key

**command:**
```bash
 echo "anything" > filename
openssl dgst -sha256 -sign filename -out filename filename
```

**example:**

```bash

┌──(syed㉿NWS23010037)-[~]
└─$ echo "lambat bayar bunga naik" > agreement.txt 

┌──(syed㉿NWS23010037)-[~]
└─$ openssl dgst -sha256 -sign private.key  -out agreement.sign agreement.txt     
```

### 2. Verify the Signature using Public Key

**command:**
```bash
openssl dgst -sha256 -verify filename -signature filename.sign filename
```

**example:**
```bash

┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-4]
└─$ cp ../task-2/public.key public.key                                             
                                                                                                                              
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-4]
└─$ ls
agreement.sign  agreement.txt  public.key
                                                                                                                              
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-4]
└─$ openssl dgst -sha256 -verify public.key -signature agreement.sign agreement.txt
Verified OK
                                                                                                                               
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-4]
└─$ 

```

### 3. Tamper the file

**command:**
```bash
echo "anything" > filename
```

**example:**

```bash
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-4]
└─$ echo "\n\n\n\n\n\nHope syed did not see this coming alsooooooo" >> agreement.txt
```



### 4. Verify again

**command:**
```bash
openssl dgst -sha256 -verify filename -signature filename filename

```

**example:**
```bash
┌──(adamriezqie㉿NWS23010043)-[~/Downloads/task-4]
└─$ openssl dgst -sha256 -verify public.key -signature agreement.sign agreement.txt
Verification failure
4067DC4F6A7F0000:error:02000068:rsa routines:ossl_rsa_verify:bad signature:../crypto/rsa/rsa_sign.c:442:
4067DC4F6A7F0000:error:1C880004:Provider routines:rsa_verify_directly:RSA lib:../providers/implementations/signature/rsa_sig.c:1041:
```

---

## problem encounted
**rsautl**: error to use rsautl for encryption,instead use **pkeyutl**


## 🔚 Conclusion

In this lab, we learned to use OpenSSL for:

Symmetric encryption with AES-256

Asymmetric encryption with RSA

File integrity verification via SHA-256 hashing

Digital signatures for message authenticity

These are essential cryptographic operations for securing data, ensuring integrity, and validating identity in modern systems.

