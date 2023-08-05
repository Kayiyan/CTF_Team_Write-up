# The third Challenge Crypto 
# Solved 

Iam sorry I do not remember this challenge and the name because it had some error with the host server of the events 

So about the solution , the title had give us the .pem key, and d , quite easy we can d this with basic script , and `chatGPT` can help us to do this too :
```python

from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import serialization, hashes
from cryptography.hazmat.primitives.asymmetric import padding

def decrypt_with_private_key(private_key_path, input_file_path, output_file_path):
    with open(private_key_path, 'rb') as key_file:
        private_key_data = key_file.read()
        private_key = serialization.load_pem_private_key(private_key_data, password=None, backend=default_backend())

    with open(input_file_path, 'rb') as input_file:
        encrypted_data = input_file.read()

    decrypted_data = private_key.decrypt(
        encrypted_data,
        padding.OAEP(
            mgf=padding.MGF1(algorithm=hashes.SHA256()),
            algorithm=hashes.SHA256(),
            label=None
        )
    )

    with open(output_file_path, 'wb') as output_file:
        output_file.write(decrypted_data)

private_key_path = 'private_key.pem'
input_file_path = 'out.enc'
output_file_path = 'decrypted_secret.txt'

decrypt_with_private_key(private_key_path, input_file_path, output_file_path)

```

After run the script we got the output is the flag at file `decrypted_secret.txt` : `ESCAPE{3C370603784112F75B70A9E2BCF1764C}`
