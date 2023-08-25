![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/241a8dcd-26de-4eab-8b68-c6ca36fdd170)# Document Trick

### _Points: 500_

### _Description:_

Người chơi lưu ý hãy tạo trước thư mục con TEST_ENCRYPT ở thư mục Desktop trước khi chạy file.

Attachment: https://drive.google.com/file/d/1ARLRk1eHwbOcBzUHKRWP15g46G0MV7Rq/view?usp=sharing

Password: infected

### _Author: yurri_re_

### _Solution:_

Mình tải attachments về và giải nén thấy có 1 folder `TEST_ENCRYPT` chứa 1 file mã hóa (như wu mình đọc được thì dựa vào đây, chúng ta sẽ phải tìm cách giải mã file đó) và 1 file bat `Tai-lieu-hoc-tap.pdf.bat`. Đọc file bat, mình thấy có một đoạn base64, mình decode và nhận được một đoạn base64 khác: 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/af67fd7a-2a14-4b7f-b047-51c26239e52d)

Mình nhận được một đoạn mã base64 và tiếp tục decode nó:

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/0f7c4288-0f6d-4a00-8a3c-26df8918bd37)

Decode xong nhận được một đoạn code powershell, theo kiến thức của mình thì mình nghĩ nó là một file powershell code và mình đã thử chạy nhưng thất bại. Mình tập trung vào đoạn mã base 64, decode thì mình nhận được một loạt dữ liệu không rõ ràng:

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/d73a5607-b4f9-4738-8085-d16d8ba854a6)

nhưng vẫn chứa một số chữ đọc được khá lạ, mình đoán đây có thể là một file.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/94fcc741-ef02-4786-b798-144b7f1f6dc1)

Mình tải file về và kiểm tra thì đây là một file PE32 (DLL).

_Sau đây là kiến thức về RE, mình không hiểu nên sử dụng đoạn [wu của một đội khác](https://github.com/bananNat/FUSec2023/blob/main/README.md)_

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/17f23012-c111-4fec-809b-323c83869e21)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/77dc5527-5f5b-4420-998c-58a92eaed979)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/ae5a5b10-25a1-498a-ba59-a6e5981b71b6)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/09a7e146-c44f-4c62-b15e-e1da7ff47c2f)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/9b4089e1-4905-4c3b-89c9-f99b6ea66885)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/00e50309-ba8a-42d4-958d-babd0bcabee5)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/393c9400-c149-4033-8469-8bc091b2259b)

_Kiến thức cần học trong phần này_

+ Sử dụng IDA cơ bản
+ Đọc code trong IDA
+ code arch x86 để chạy file DLL

> Flag: `FUSec{b6ffcf2ef6bf4f1a0debe2fd591992ade0597c05f49dfdd66a6377009217fe41}`
