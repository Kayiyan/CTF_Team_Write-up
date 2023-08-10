# Name : Rick
# Points : 116


# Description #
i lost my flag in this thing :(

[rick](http://34.29.19.233/dl/?rev/rick/rick)

# Solved #

Sau khi tải file về, mình kiểm tra file và nó là file nhị phân, dùng IDA64 để disassemble và chuyển về pseudocode để dễ đọc thì được như sau

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/68564c72-a730-437f-8d6a-770530b2c2c2)

Chương trình in ra "wat is flag"

Vì vậy dữ liệu bên trong file sẽ có nghĩa và chúng ta cần giải mã nó. Mình tìm được một công cụ để decode là [WhiteSpace Language](![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/9b668cea-7891-4dd4-9071-071c04543556)
). Sau khi sử dụng, mình thu được một đoạn text.
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/a5b7a6fb-eb06-4188-a047-a8a954b9a47c)

Nhiệm vụ cuối cùng là bọc nó trong LITCTF{}.

# Flag #

`LITCTF{h1d1ng_1n_pl41n_s1ght}`
