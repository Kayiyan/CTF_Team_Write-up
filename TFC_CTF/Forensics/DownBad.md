# Name : DOWN BAD
# Level : WARMUP
# Points : 50

# Description #

The flag is right there!<br>
File: [down_bad.png](https://drive.google.com/file/d/1k16yKbV_6fgrSFjg0GVuEVWDvqwCPpok/view)

# Solved #

Việc đầu tiên mình làm sau khi tải file xuống là kiểm tra file đó là file gì, 
sau khi xác nhận đó là file **png** thì mình sử dụng lệnh pngcheck để kiểm tra file ảnh có vấn đề gì không 
thì phát hiện file ảnh có lỗi.

![image](https://github.com/Kayiyan/CTF/assets/112896213/4db97471-271d-4599-86b1-2b348ff4ed8e)

Và lỗi là ở chunk IHDR. Mình tìm hiểu thêm về chunk IHDR thì nhận thấy offset của bytes CRC of chunk's type and content (but not length) là 21

![image](https://github.com/Kayiyan/CTF/assets/112896213/26aa2a69-bc07-479d-85b7-807d6d41f964)

![image](https://github.com/Kayiyan/CTF/assets/112896213/29ecfa58-47a6-4361-9da6-206101429026)

![image](https://github.com/Kayiyan/CTF/assets/112896213/28e5342f-d05f-4228-bf16-1a3f6bc96eb5)

Vậy là bây giờ mình cần chỉnh sửa byte này để file png này không bị lỗi nữa.
Tại offset 21, hex value là 0xA9D5455B, mình nhận thấy nó giống với ERROR đã được thông báo nên mình thay đổi hexvalue này thành 0x1d9c52c0 và hi vọng nó sẽ fix được.

![image](https://github.com/Kayiyan/CTF_Team/assets/112896213/16058982-1cb9-4aaf-82bb-1ddf5b5e83f1)

Sau khi sửa được lỗi bằng hexed.it, mình kiểm tra lại file thì nó không có lỗi.

![image](https://github.com/Kayiyan/CTF_Team/assets/112896213/4f8512d8-7d26-422e-8c91-946b796b8ff2)

Mình kiểm tra bằng các lệnh exiftool, zsteg và chỉnh size ảnh thành hình vuông thì thu được flag.
(Chỉnh ảnh thành hình vuông có chiều rộng = chiều dài).

![image](https://github.com/Kayiyan/CTF_Team/assets/112896213/b11d3b9f-5521-4dbe-9eac-1734330ea610)

Flag: `TFCCTF{28ae25c96850245ffdd70a880158f9f3}`

