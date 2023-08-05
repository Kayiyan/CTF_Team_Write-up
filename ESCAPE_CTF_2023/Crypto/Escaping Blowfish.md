# Name : Escaping Blowfish
* Points : 150
* Category : Cryptography
# Description 

I found a strange part while analyzing the enemy's communication! Can you tell me what it is?

Author : Sy2n0 Points 150

# Solution
This chall is about Blowfish cipher ,the link give us the cipher text and the source file  given the key 

I think we have all we need to decrypt the cipher text , let write the basic script to decrypt it : 

```python
from Crypto.Cipher import Blowfish
from Crypto.Util.Padding import unpad

key = b"N0bOdy_cAn_FiNd_Th1s!"  
ciphertext = b'\x0b\xfc\x8d\xcb\x0eE\x9cdh\x8br8\x14\xb0HV\xd6M\xb2\xda\x88L\xe3"\xeb\xb6\xafS\xd4[\xb1S\x93\xda\xcdF;\x19\xb9\xc5x\xe2D\xad\x88g\xb5\x82\xa4\xe3X\xeb\x02\x15\xdf\xee'

cipher = Blowfish.new(key, Blowfish.MODE_ECB)

# Giải mã ciphertext
padded_plaintext = cipher.decrypt(ciphertext)

# Loại bỏ padding để nhận lại thông điệp ban đầu
plaintext = unpad(padded_plaintext, 8)

print(plaintext)

```

Run the script then got the flag we need :

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/4a969135-d56e-4570-afc6-d667e8c61302)



