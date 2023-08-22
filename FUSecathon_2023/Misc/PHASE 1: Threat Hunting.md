# PHASE 1: Threat Hunting

### _Points: 419_

### _Description:_

Bạn được giao nhiệm vụ săn tìm mã độc ẩn trong một máy window 10 chứa (nhiều) dữ liệu (quan trọng). Flag chính là tên file (không chứa path) của mẫu mã độc bạn cần tìm. Nội dung của flag không chứa kí tự viết hoa. Dưới đây là [link tải file máy ảo](https://drive.google.com/file/d/16ntnmYhpmU67uPMcxB_p2OcRRPCqjSUC/view?usp=sharing) (bỏ vào Virtualbox để chạy). Mật khẩu vào máy ảo là "123".

Cần giải bài này trước khi giải Phase 2: Malware analysis. 

### _Author: EaZyq_

### _Solution:_

Mình sử dụng Virtual Box và mở máy ảo lên thì thấy xuất hiện rất nhiều ứng dụng, có vẻ như rất nhiều phần mềm được chạy khi khởi động.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/ad1dd80c-d4eb-47b2-97e3-233870761c26)

Nhìn sơ qua các ứng dụng đang được mở thì là các ứng dụng đáng tin cậy (các phần mềm thường được sử dụng). Có thể mã độc đã được ẩn đi. 
Vì vậy để tìm ra mã độc, mình sẽ kiểm tra các process chạy tự động. 

Mình sử dụng `autorun.exe` để tìm các process đang chạy, vì tất cả các process có thể ẩn nhưng nó vẫn phải chạy và đều được ghi lại. 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/746edfdd-c802-4e25-8e24-595fef95e8dd)

Ở đây, mình chú ý hơn cả vào những dòng có màu đỏ (Not Vertified), và nhận thấy Entry `Red Apple` là entry đáng nghi.

> **Flag**: `FUSec{svchost.exe}`
