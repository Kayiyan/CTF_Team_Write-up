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

* Theo form flag của bài này và thu được : `CACI{GaleBoetticher}`

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

* Flag thu được : `Flag{python_is_e@sy}`

# Patchwork #
* Description
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/170517ed-d2f9-4ec9-9ac8-03811246d261)

* Solution
Kiểm tra qua file cung cấp , đây có cấu trúc 64bit và yêu cầu cần dịch ngược để tìm flag :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/806b0aee-c1f2-4498-b80b-b36e945a2762)

Cụ thể là cần nhảy đến 1 hàm khác.

Decomplie file bằng `IDA` hoặc `Ghidra` , nhận thấy gọi được hàm `give_flag()` là được.

Thực hiện debug với `Gdb-peda`:

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/a22cad19-e8d7-4468-ba98-a6ce3aca8352)


Tiến hành đặt break point tại main+8 :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/319ab957-eedf-4b91-a418-6da4c3f42290)

Thực hiện `n` để chạy tiếp các lệnh tiếp cho đến  khi gặp lệnh `cmp    DWORD PTR [rbp-0x4],0x0` :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/d0088573-0ad7-467a-96b2-4b9aa537ffcd)

lệnh thực hiện so sánh giữa giá trị của biến nằm tại địa chỉ [rbp-0x4] trong bộ nhớ và giá trị 0.

Ngay sau đó là lệnh `je 0x555555555176 <main+61>` ,một lệnh điều kiện sử dụng để kiểm tra kết quả của phép so sánh trước đó và thực hiện một nhảy (jump) tới địa chỉ 0x555555555176 nếu kết quả của phép so sánh là bằng (equal). 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/0acc0c8d-c53a-4708-b0f8-db47a4126201)

Tiến hành đặt break point tại `main+45` và chỉnh sửa giá trị của rbp-0x4 sao cho khác 0 là được để câu lệnh jump equal ko thực hiện:

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/40791540-5101-4c60-9beb-6226f4789e5b)

Tiếp tục `next` để chạy đến hàm `give_flag()` :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/294672e9-aed0-49cd-901f-128ab3300d47)

* Flag thu được : `PCTF{JuMp_uP_4nd_g3t_d0Wn}`



# Suboptimal #
* Description

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/b8f5bbe9-85e9-48b0-a0df-128111e5316b)

* Solution

Kiểm tra qua , file có cấu trúc 64 bit : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/e618e4b5-ff6a-42b6-8eea-7d5f919d0ea8)


Mở file với `IDA` , Hàm `Main` : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/0f4a8d39-5705-46f2-b99b-1105d44b4df9)

Code xử lí 1 vòng lặp for và thực hiện encode qua 2 hàm complex, đọc qua 2 hàm này chúng có vẻ tương tự nhau nhưng hàm complex 1 có 1 chút khác :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/16d2a453-cf18-4ebf-ac15-0eaf5b69caa3)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/b6c7fc39-9cc2-40ea-ac19-bfa439c578d4)


2 Hàm giống nhau về thuật toán ngoài ra hàm complex còn cho ta biết độ dài, các kí tự đi vào hàm complex thì không đụng chạm đến nhau, ở đây sẽ thực hiện bruteforce đến khi nào bằng với `length` cho thì dừng :

```C
#include<stdio.h>
int main(){
    //char flag[] = "pctf{simproc_r_optimal}";
    char flag[] = "pctf{thisisafakeflagka}";
    char test[] = "xk|nF{quxzwkgzgwx|quitH";
    int size = sizeof(flag);
    int i;
    char tmp, j;
    for(i = 0; i < 24; i++){
        for (j = 'A'; j <= '}'; j++) {
            tmp = j;
            tmp = (tmp + 65) - 122;
            if(tmp <= '@'){
                tmp += 61;
            }
            tmp = (tmp + 65) - 122;
            if(tmp <= '@'){
                tmp += 61;
            }
            if(tmp == test[i]){
                putchar(j);
                break;
            }
        }
    }
    putchar('\n');
    return 0;
}

```
* Flag thu được : `pctf{simproc_r_optimal}`










  
