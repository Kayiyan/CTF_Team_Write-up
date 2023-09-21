![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/ec7aa156-11fc-443d-ae1e-b7d0f50d323c)# Impossibrawler!
# Description :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/66a1117d-f370-406d-a8e6-10b2f363077a)

# Solution:

Bài cung cấp 2 file : `.exe` và `.pck` :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/6a0608d1-8861-49c4-a98f-d66867fc840e)

Kiểm tra qua thử và có thể mở game lên để xem qua thì game này được viết bằng godot engine , từ đây ta có thể tìm kiếm các công cụ để dịch ngược godot bởi đơn giản là win the game là có thể lấy được flag từ bài nhưng player trong game không thể đánh quái vật bằng cách thông thường.

[Link to tool](https://github.com/bruvzg/gdsdecomp/releases)

Sử dụng `Godot RE Tools` mình cung cấp ở trên để lấy được `source code` của game : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/17331658-0157-47de-a0d4-f3698a7442c7)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/7018823b-6b36-41db-9b63-e335112cbfef)

Okay sau khi phục hồi lại và lấy được source code , hãy chú ý vào thư mục script, ở đây có dữ liệu về game, các hàm và file dữ liệu của level, player,... : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/ed85912e-20bf-4a7e-b4ec-b6b65db2bd8c)

Ngoài ra file `Vals.sd` cũng cần chú ý vì lát nữa sẽ cần đến : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/120e355f-50cc-42bd-9fab-bf8ca5d61ca3)

Script của `level 2`  :

```godot

extends Node2D

var totalenemies = 0
var rng = RandomNumberGenerator.new()
var enemies_left = 0


func _process(delta):
	var mousepos = get_global_mouse_position()
	get_node("Crosshair").position = mousepos

	if enemies_left == 0:
		rng.seed = int(Vals.sd)
		var fbytes = rng.randf()
		Vals.sd = fbytes
		fbytes = str(fbytes)
		var flg = fbytes.to_ascii().hex_encode()
		$CanvasLayer / Label.set_text("csawctf{" + flg + "}")

func _on_Enemy_killed():
	enemies_left -= 1

func _on_Enemy_alive():
	totalenemies += 1
	enemies_left += 1

func _ready():

	Input.set_mouse_mode(Input.MOUSE_MODE_HIDDEN)

```

Ở đây cho biết khi kẻ thù = 0 , giá trị seed sẽ được phạm vi của trình tạo số ngẫu nhiên được gieo với giá trị thu được từ int(Vals.sd) sau đó ngẫu nhiên được tạo, fbyte được chuyển thành chuỗi. Tiếp đó là encode với hex gán vào biến flg tương ứng cho flag và in ra cùng với form flag 

Ban đầu `Vals.sd` đã được khởi tạo bằng 0 và từ level 1 script có thể thấy fbytes sẽ lưu vào Vals.sd khi kẻ thù = 0 :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/c4a8a750-0f9e-4b39-9c7b-42d3ff7a51ef)

Ngoài ra ,hàm randf() trả về các giá trị trong khoảng từ 0 đến 1, do đó, việc vals.sd có được cập nhật hay không không quan trọng vì ở cấp_2, nó được chuyển thành kiểu int khiến nó trở thành 0.

Vì vậy , không cần thiết phải build lại game , ta có thể lấy được flag qua chạy script online :

```godot
extends Node

func _ready():
    var rng = RandomNumberGenerator.new()

    var sd = 0;
    rng.seed = int(sd)
    var fbytes = rng.randf()
    sd = fbytes
    fbytes = str(fbytes)
    var flg = fbytes.to_ascii().hex_encode()
    print("csawctf{" + flg + "}")
```

Sử dụng [gdscript online](https://gd.tumeo.space/?KYDwLsB2AmDOAEA5A9tYAodAzArpAxvAPoBOwAhtAJ4AUAlAFzrwvwBu5J8JkA5vAF54AJXIxkAW0Q4JAI2AkA4lAXkwyEgDpIwAO71Mrdp3ixog+AAYA3M1Y9em2MGDmhAS0hgaZunZYcXFiyVBAIQg6aJGLQWAZGZhbBocCw-vDJYRawYCQ0mal+RoEZADb8QgWwmupE5LD47u70mgAWoERQ+KjA8awADiSe3gBE+LDkuvhgWADeI-AA1GX8yyMAviN0QA) để thực hiện.

* Flag : `csawctf{302e323032323732}`

