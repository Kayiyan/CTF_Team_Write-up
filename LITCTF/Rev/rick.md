# Name : Rick
# Points : 116


# Description #
i lost my flag in this thing :(

[rick](http://34.29.19.233/dl/?rev/rick/rick)

# Solved #

Sau khi tải file về, mình kiểm tra file và nó là file nhị phân, dùng IDA64 để disassemble và chuyển về pseudocode để dễ đọc thì được như sau

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/68564c72-a730-437f-8d6a-770530b2c2c2)

Chương trình in ra `wat is flag` và nhận 63 kí tự vào xâu s, có vẻ là sẽ nhập Flag vào và kiểm tra xem flag có đúng hay không. Sau đó dùng for để đảo ngược xâu flag nhập vào rồi copy `a1l0rkc1r7xen3h` vào `dest` bằng lệnh memcpy. Rồi dùng `strcmp` để so sánh flag đã đảo ngược với `dest`. Vì vậy ta sẽ xem `a1l0rkc1r7xen3h` có gì.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/82ad21c4-3cbe-4260-86bc-ad166292bcb9)

Và đúng thật `a1l0rkc1r7xen3h` chứa flag đảo ngược `}1l0rkc1r_7xen_3ht_3k4m_4nn0g_7pgt4hc{FTCTILse 7)`, vì vậy ta chỉ cần dùng công cụ `CyberChef` để đảo ngược lại xâu trên và nhận được flag như hình.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/b8ce4e5f-1505-4c21-abf9-d66d6386679d)

# Flag #

`LITCTF{ch4tgp7_g0nn4_m4k3_th3_nex7_r1ckr0l1}`
