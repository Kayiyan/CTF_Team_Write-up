# Name : Blank and Empty
# Points : 122

# Description #

This file seems blank, but it's hiding some secrets. (Wrap the flag in LITCTF{}) <br>
[blank.txt](http://34.29.19.233/dl/?misc/blank-and-empty/blank.txt)

# Solved #

Sau khi tải file về, mình kiểm tra file và nhận thấy đây là một file ASCII có dữ liệu, không phải file empty.
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/0c6191bb-f244-48a9-872e-2624361be4ad)

Vì vậy dữ liệu bên trong file sẽ có nghĩa và chúng ta cần giải mã nó. Mình tìm được một công cụ để decode là [WhiteSpace Language](![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/9b668cea-7891-4dd4-9071-071c04543556)
). Sau khi sử dụng, mình thu được một đoạn text.
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/a5b7a6fb-eb06-4188-a047-a8a954b9a47c)

Nhiệm vụ cuối cùng là bọc nó trong LITCTF{}.

# Flag #

`LITCTF{h1d1ng_1n_pl41n_s1ght}`
