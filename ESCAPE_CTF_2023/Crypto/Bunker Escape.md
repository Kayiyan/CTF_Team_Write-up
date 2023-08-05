# Name : Bunker Esacape
* Points : 150
* Category : Cryptography
# Description

During the infiltration operation into the bunker, while attempting to escape, we were discovered by the enemy, and all the bunker's doors were shut...! Fortunately, you were still able to intercept the enemy's communication. Decode the communication to gather information!

Author : suriryuk Points: 150

# Solution 

From file of description i realize this is the RSA encryption : 

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/9d41c58a-daaa-40e0-81a7-bb209ef9d8e9)

Okay from RSA math have many algorithms , in this we have `n` and `e` and from the first ip connect to the server give us the 4 values of `c` which is `ciphertext` we need to decrypt.

At least must have `d` to caculate so let do that , follow this : `d =e^-1(mod n-1)`

In here I using `sagemath` to caculate , it quite cool because it use python syntax :

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/be9d8a5d-2c93-4841-9aa3-51b197cce5f3)

OKay after got `d` let cacalate to find the message from each 4 number `c` follow this : ` message = c^d mod n` ( m = c^d mod n)

I write this basic script to caculate that and print out all value of message :

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/73d984a3-6133-4167-b2de-14f7ad804524)

Run the script then got the password which need to check in the seccond ip connect : 

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/4eaf52f2-3be2-455b-af65-f7c8f614757e)

OKay got the password , then type it in the seccond ip connect and got the flag : `ESCAPE{Escape_enemy_bunker!!_you_live!}`
