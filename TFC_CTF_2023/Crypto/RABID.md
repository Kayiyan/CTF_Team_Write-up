# RABID
# Level : Easy
# Points : 50

# Description #

There might be a little extra piece of information here.

# Solved #

At the first , by looking at the code given in the file `rabit.txt` : 

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/aade6db4-2903-45aa-9da3-7a70b9084ac6)

it look like `base64` encoded , let try decode it in [Cyberchef](https://gchq.github.io/CyberChef/) :

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/7fdfe205-5e8e-4e77-bfa3-a930439e8d33)

It seems have some piece of the flag : 

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/eecaad6a-1081-491c-b625-165b2fae32d3)

Okay so from experiance , let try delete some character , it might figure out some things : 

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/1f9151e7-e0d2-4240-b765-d25777d2c624)

Humm, this might related to the flag we need but i got flag when submit it : `×y0u_r4b1d_d0g?!?!?!?!?!>/>?>?>12390jcapskdj091randomlettersandnumbersreeee2j3}` 

A little bit tricky is the word have meaning so let delete the 
`extra` characters like extra from the Description information 

Finnally , the correct flag is : `TFCCTF{y0u_r4b1d_d0g?!?!?!?!?!>/>?>?>12390jcapskdj091randomlettersandnumbersreeee2j3}`


