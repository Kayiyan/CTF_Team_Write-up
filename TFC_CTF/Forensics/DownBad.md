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
