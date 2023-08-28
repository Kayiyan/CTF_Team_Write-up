# Name: Azusawa’s Gacha World
# Point: 100

# Description #
https://azusawa.world/#/2023/03/02

Author: enscribe

# Note
The website only contains the challenge description, and is not needed to solve the challenge.
https://storage.googleapis.com/sekaictf-2023/azusawa/dist.zip

# Solved #
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/6dd6a955-1655-463d-b75d-df8f3aa5a233)

Là 1 tự game Gacha làm bằng Unity với tỉ lệ ra nhân vật 4*, là 1 phần triệu (dòng màu đỏ hồng). Nên có thể chúng ta phải quay 1 triệu lần mới có thể xuất hiện flag.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/8c9f1f42-6912-4e85-8db8-80fb3b0cd461)

## Cách 1: sửa lại source game bằng dnSpy
Dùng [dnSpy](https://github.com/dnSpy/dnSpy/releases) dịch ngược lại file `Assembly-CSharp.dll`. Ta cần quan tâm các class liên quan đến cách gacha như `GachaManager`, `GameState`, `UIManager`.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/62005f03-0269-409a-b819-7475eaee5935)

Bắt đầu từ `GachaManager`
Class `SendGachaRequest` gửi 1 JSON gồm crystal, pulls, numpulls bằng cách tạo 1 JSON qua class `GachaRequest`

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/0fd27c2b-6713-4b5b-b18d-113422f39bd9)

Trong class `GachaRequest` trả về crystals, pulls, numPulls. pulls là số lần mình đã gacha nên mình thử đổi thành 999.999 xem sao (vì mới bắt đầu game đã có sẵn 100 crystal và đủ để quay thêm 1 lượt nữa). Nên chỉ cần số pulls hiện tại là 999.999 quay thêm 1 nữa là vừa đủ 1 triệu.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/b5a0a733-a146-48a0-9a73-41b58d9dd37a)

Và với cách này nó thực sự hoạt động

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/7070b197-cc41-4eeb-86bd-c1a94d5d70a9)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/78b4084d-b53d-4f31-b41b-50fc9a8df369)

## Cách 2: gửi pulls request bằng python

Tiếp tục từ class `SendGachaRequest`, sau khi đã tạo được JSON thì JSON này được gửi bằng class `CreateGachaWebRequest`
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/7783c48f-8d3a-45f6-8bca-bbef46c9c7c1)

Pulls request được gửi đến `string s = "aHR0cDovLzE3Mi44Ni42NC44OTozMDAwL2dhY2hh";` đã được encode sang base64. Decode thì ta nhận được 1 URL là `http://172.86.64.89:3000/gacha`
 với các header 
 
```
"Content-Type", "application/json"
"User-Agent", "SekaiCTF"
```
kèm theo JSON data đã được tạo lúc nãy.
Ta có thể tạo được request như bên dưới, ta cũng sửa số pulls thành 999.999 để chỉ cần request 1 lần là nhận được nhân vật 4*

```
headers = {
    "Accept": "*/*",
    "Accept-Encoding": "deflate, gzip",
    "Content-Type": "application/json",
    "User-Agent": "SekaiCTF",
    "X-Unity-Version": "2021.3.29f1"
}

data_template = {
    "crystals": 100,
    "pulls": 999999,
    "numPulls": 1
}
```

Tại class `UIManager` có hàm `DisplayFourStarCharacter` nhận vào nhân vật có chứa thuộc tính `flag` đã được encode sang base64 nên tác giả đã decode từ base64 và nhận được 1 file ảnh PNG. 
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/640f2871-c9f5-4e98-94c0-0405a0441a0d)

Vì vậy sau khi nhận được response khi gửi pulls request. Mình sẽ vào đối tượng `characters` và lấy thuộc tính `flag` mang đi decode từ base64 và nhận được flag

![flag](https://github.com/Kayiyan/CTF_Team_Write-up/assets/60804710/e945dfc5-2cb9-40e1-920b-9c1823145bdd)

Source code 
```python3
import requests
import json
import base64
from PIL import Image
import io

url = "http://172.86.64.89:3000/gacha"

headers = {
    "Accept": "*/*",
    "Accept-Encoding": "deflate, gzip",
    "Content-Type": "application/json",
    "User-Agent": "SekaiCTF",
    "X-Unity-Version": "2021.3.29f1"
}

data_template = {
    "crystals": 100,
    "pulls": 999999,
    "numPulls": 1
}

response = requests.post(url, headers=headers, json=data_template)
response_data = response.json()

if "characters" in response_data:
    # Lấy đối tượng character đầu tiên trong danh sách
    character = response_data["characters"][0]
    if "flag" in character:
        flag_base64 = character["flag"]
        flag_bytes = base64.b64decode(flag_base64)

        # Tạo đối tượng hình ảnh từ dữ liệu nhị phân
        image = Image.open(io.BytesIO(flag_bytes))

        # Lưu hình ảnh
        image.save("flag.png")

        # Mở hình ảnh
        image.show()
    else:
        print("No flag field found in the character")
else:
    print("No characters field found in the response")

```

# Flag #

`SEKAI{D0N7_73LL_53G4_1_C0P13D_7H31R_G4M3}`
