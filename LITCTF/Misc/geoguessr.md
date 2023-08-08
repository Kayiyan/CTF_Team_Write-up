# Name : geoguessr
# Points : 165

# Description #

Where am I? The flag is LITCTF{latitude,longtitude} rounded to the third decimal place. (Example: LITCTF{42.444,-71.230}).
[geoguesser.png](http://34.29.19.233/dl/?misc/geoguessr/geoguessr.png)

# Solved #

Điểm đầu tiên mà mình chú ý trong bức ảnh là hai bức tường chống ồn ở 2 bên đường, cùng với giá treo biển báo của con đường này.
Nhưng dựa vào đó, mình không thể tìm được ra bất cứ nơi nào có bức tường chống ồn hay kiểu biển báo như vậy.

Vì thế mình chú ý đến các chi tiết nhỏ hơn như con đường này là một xa lộ, nó sử dụng một loại biển báo đặc biệt và vạch kẻ đường có màu đen trắng xen kẽ. <br>
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/4207810b-956d-4a2a-8a9e-127b1e2ebcde)
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/e61608f9-e56a-4ef1-97c0-6ebf84c73288) <br>
Và đây là biển báo được sử dụng ở phía Nam Mỹ. Mình nhận ra 2 con số, 1 số là số có 2 chữ số và 1 số là số có 3 chữ số.

Phỏng đoán ban đầu của mình thì số có 2 chữ số kia có thể kết thúc bằng 1 hoặc 7. Mình đã đi tìm các con đường như 61 71 91 81 87.

Con đường này có tốc độ tối đa là 55mph, thường thì một xa lộ sẽ không giới hạn tốc độ di chuyển này, 
nên mình nghĩ đoạn đường này sẽ là một giao lộ hoặc đầu vào của xa lộ. Hơn nữa, con đường này có giải phân cách và có phản quang màu vàng.
Đặc biệt, mình còn phát hiện ra cột đèn ở phía bên trái rất cao, và mình nhận ra đoạn đường này là một cây cầu bắc qua một con đường hoặc một con sông.
Đó sẽ là những đặc điểm để mình tìm kiếm trên google earth mà không cần xem qua street view quá nhiều.

Mình sẽ xem xem xa lộ nào là xa lộ có điểm chung trong ảnh. Mình xem các đoạn đường 61 71 91 81 thì nhận thấy giải phân cách không có phản quang giống với trong hình,
hoặc là các nút giao không có số làn giống như mình mong muốn, không phải là cầy cầu bên trên, không có thanh treo bắc qua 2 làn đường, ... mình sẽ
so sánh những đặc điểm ấy để biết xa lộ đang xem xét phải xa lộ cần tìm không.

Và xa lộ 87 là xa lộ duy nhất phù hợp. Mình men theo xa lộ và tìm ở các điểm giao có vạch kẻ đường đen trắng là tìm được vị trí.
![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/bf906db8-7cb0-4ff9-a390-b456b857fa50)

# Flag #

`LITCTF{41.077,-73.921}`
