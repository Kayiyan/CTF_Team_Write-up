# Name : Hidden 
# Description : 
Find the flag hidden in the picture
# Solution

Check the file from the beginning , reliaze this is the pcap file , so change it to right type then open with wireshark

After that we need extract the png file from `export object` options :

I will check the `http` protocol and saw the `png` file : 

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/e4cb174c-36fa-4d2b-bd45-18b9dfa66ccd)

Export that might find out somethings 

The picture i got :

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/b3de0d6f-3750-4c2a-9c93-8c765344686c)

So close to the flag , now open stegsolve to modify some plane bit to make it more clearly and got the flag :

![image](https://github.com/Kayiyan/CTF_Team/assets/126185640/d9ad9997-6fd2-4ca5-bfc4-4c6f6eb43c1f)


The flag i got : `ESCAPE{H1dD3n_!n_7he_P1ctur3_1N_TeXt}`


