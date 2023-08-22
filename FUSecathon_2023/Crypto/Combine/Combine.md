# Combine

Here the article uses 2 algorithms RSA and AES, from the first comment I see that the value of `e` is very small in the `RSA` algorithm `( e = 3)` along with that `c` is also much smaller than `n` so it can be Quickly calculate the value of the key used to decrypt the `AES` algorithm behind

`key = secret#keysummerSuperSecureAyyah` this key can caculate from tool online like [decode.fr](https://www.dcode.fr/rsa-cipher) or basic script 

After get the key , continue to decrypt AES :

```python

from Crypto.Cipher import AES

# AES decryption function
def decrypt_message(key, ciphertext):
    cipher = AES.new(key, AES.MODE_ECB)
    plaintext = cipher.decrypt(bytes.fromhex(ciphertext)).rstrip(b"\0").decode()
    return plaintext

# Ciphertext and key
ciphertext = 'e6c2921a3edb52639e871ebad04f16ff4580870a8522295cf58914b09fee749afcdd94a0beb8471dbaa50ed37693653295d4e798798674e2048f5c233cd9aba1'
key = b'secret#keysummerSuperSecureAyyah'  # Convert the key to bytes

# Decrypt ciphertext using the key
decrypted_message = decrypt_message(key, ciphertext)

# Print the decrypted message
print("Decrypted Message:", decrypted_message)

```
