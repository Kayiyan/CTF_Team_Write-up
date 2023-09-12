# Coffee Shop # 
* Description :
  
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/e08b8240-d155-434e-98ac-fa838e698c12)

* Solution

Giải nén file zip bài cung cấp , giải nén và nhận được 1 file `CoffeShop.jar` :

File `java` này đã bị complie nên hay decomplie lại nó qua `jd-gui` để xem mã :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/a4bdac13-120d-489e-a7fd-8bfe58e2ed38)

Xem qua code của bài , nhận thấy rằng `str1` nhận đầu vào từ người dùng sau đó `str2` lấy nó và encode qua base64 và kiểm tra thông qua khối lệnh trong `if`:

```java
    if (str2.length() != 20) {
      failure();
    } else if (!str2.endsWith("NoZXI=")) {
      failure();
    } else if (!str2.startsWith("R2FsZU")) {
      failure();
    } else if (!str2.substring(6, 14).equals("JvZXR0aW")) {
      failure();
    } else {
      success();
    } 
  }
```
Dựa vào đây có thể thấy nếu `str2` thỏa mãn vòng `else` cuối -> success và có thể đây là `flag` hoặc thông tin quan trọng :

Ghép các giá trị lại : `R2FsZUJvZXR0aWNoZXI=` theo các điều kiện và decode qua base64 : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/85d6ab62-f789-415e-8b10-6f9deb2f3e40)

Theo form flag của bài này và thu được : `CACI{GaleBoetticher}`

# Python XOR #
* Description :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/bfb11fe9-1c4e-4f67-8c7b-77a698f201eb)

* Solution

Bài cung cấp 1 file python với nội dung đã encrypt flag và cần tìm lại qua việc lặp qua các bảng chữ cái :

```python
from string import punctuation

alphabet = list(punctuation)
data = "bHEC_T]PLKJ{MW{AdW]Y"
def main():
#   For loop goes here
    key = ('')
    decrypted = ''.join([chr(ord(x) ^ ord(key)) for x in data])
    print(decrypted)
main()
```
ở `decrypt` đã xor key với từng giá trị trong data (flag đã encrypt) , cần có key và xor lại là được.

Với kinh nghiệm thì việc đầu tiên mình sẽ xor lại với form flag mà bài cung cấp bởi vì bài cung cấp khá ít dữ kiện và điều kiện thì đơn giản , hàm decrypt họ đã làm cho sẵn và việc của mình là chỉ cần key :

```from pwn import xor
from string import punctuation

alphabet = list(punctuation)
data = "bHEC_T]PLKJ{MW{AdW]Y"

def main():
    target_prefix = "Flag{"
    
    for key in alphabet:
        key_bytes = bytes(key, 'utf-8')
        decrypted = xor(data.encode('utf-8'), key_bytes).decode('utf-8')
        if decrypted.startswith(target_prefix):
            print(f"Found key: {key}")
            print(f"Decrypted data: {decrypted}")
            break

main()
```

Flag thu được : `Flag{python_is_e@sy}`

# Patchwork #
* Description
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/170517ed-d2f9-4ec9-9ac8-03811246d261)

* Solution
Kiểm tra qua file cung cấp , đây có cấu trúc 64bit và yêu cầu cần dịch ngược để tìm flag :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/806b0aee-c1f2-4498-b80b-b36e945a2762)

Cụ thể là cần nhảy đến 1 hàm khác.

Decomplie file bằng `IDA` hoặc `Ghidra` , nhận thấy gọi được hàm `give_flag()` là được :















  
