# Name : MAYDAY!
# Level : WARMUP
# Points : 50
![image](https://github.com/Kayiyan/CTF/assets/126185640/114fbb52-7fa9-49ea-9808-6ea22dc59ae4)

# Description #

We are sinking! The nearest ship got our SOS call, but they replied in pure gobbledygook! Are ye savvy enough to decode the message, or will we be sleepin' with the fish tonight? All hands on deck!

Whiskey Hotel Four Tango Dash Alpha Romeo Three Dash Yankee Oscar Uniform Dash Sierra One November Kilo India November Golf Dash Four Bravo Zero Uniform Seven

Flag format: TFCCTF{RESUL7-H3R3}

# Solved #
At the first , the message does not contain anything so look at the string below that much better : 

![image](https://github.com/Kayiyan/CTF/assets/126185640/1e197d6f-f9a7-41a0-bdfd-173c6aedc397)

Okay so after looking around and by searching it i found this is the [Alpha bravo alphabet](https://jakubmarian.com/alpha-bravo-charlie-what-is-it/#:~:text=Briefly%20put%2C%20Alpha%2C%20Bravo,called%20the%20NATO%20phonetic%20alphabet.) 

Okay cool it very close right now, let use the dcode.fr tool to decode it , you guys can search [Nato decode](https://www.dcode.fr/nato-phonetic-alphabet) to finding it : 

![image](https://github.com/Kayiyan/CTF/assets/126185640/72828712-a5ca-437d-b561-d3f71c838970)

Okay so i got the flag : `TFCCTF{WH4T—AR3—YOU—S1NKING—4B0U7}`
