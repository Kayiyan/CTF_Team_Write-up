# Description 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/282bd20c-4c45-4eed-bf54-00cba0229d75)

# Solved 

Bài chỉ cung cấp 1 file `.eml` và mình cần phân tích dữ liệu quan trọng có được từ file , ngay từ đầu mình thử đọc file và đáng chú ý ở gần cuối của file chứa 1 đoạn mã `base64` trông đáng ngờ :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/c0dacb5b-7229-4a07-b08f-d33aeeae692b)

[link decode trên cyberchef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=UWtWSFNVNDZWa05CVEVWT1JFRlNEUXBXUlZKVFNVOU9Pakl1TUEwS1VGSlBSRWxFT2kwdkwybGpZV3d1YldGeWRXUnZkQzVqYjIwdg0KTDJsRFlXd2dSWFpsYm5RZ1RXRnJaWElOQ2tOQlRGTkRRVXhGT2tkU1JVZFBVa2xCVGcwS1FrVkhTVTQ2VmxSSlRVVmFUMDVGRFFwVQ0KV2tsRU9rRnRaWEpwWTJFdlRHOXpYMEZ1WjJWc1pYTU5Da3hCVTFRdFRVOUVTVVpKUlVRNk1qQXlNREV3TVRGVU1ERTFPVEV4V2cwSw0KVkZwVlVrdzZhSFIwY0RvdkwzUjZkWEpzTG05eVp5OTZiMjVsYVc1bWJ5MXZkWFJzYjI5ckwwRnRaWEpwWTJFdlRHOXpYMEZ1WjJWcw0KWlhNTkNsZ3RURWxETFV4UFEwRlVTVTlPT2tGdFpYSnBZMkV2VEc5elgwRnVaMlZzWlhNTkNrSkZSMGxPT2tSQldVeEpSMGhVRFFwVQ0KV2s1QlRVVTZVRVJVRFFwVVdrOUdSbE5GVkVaU1QwMDZMVEE0TURBTkNsUmFUMFpHVTBWVVZFODZMVEEzTURBTkNrUlVVMVJCVWxRNg0KTVRrM01EQXpNRGhVTURJd01EQXdEUXBTVWxWTVJUcEdVa1ZSUFZsRlFWSk1XVHRDV1UxUFRsUklQVE03UWxsRVFWazlNbE5WRFFwRg0KVGtRNlJFRlpURWxIU0ZRTkNrSkZSMGxPT2xOVVFVNUVRVkpFRFFwVVdrNUJUVVU2VUZOVURRcFVXazlHUmxORlZFWlNUMDA2TFRBMw0KTURBTkNsUmFUMFpHVTBWVVZFODZMVEE0TURBTkNrUlVVMVJCVWxRNk1UazNNREV4TURGVU1ESXdNREF3RFFwU1VsVk1SVHBHVWtWUg0KUFZsRlFWSk1XVHRDV1UxUFRsUklQVEV4TzBKWlJFRlpQVEZUVlEwS1JVNUVPbE5VUVU1RVFWSkVEUXBGVGtRNlZsUkpUVVZhVDA1Rg0KRFFwQ1JVZEpUanBXUlZaRlRsUU5Da1JVVTFSQlRWQTZNakF5TXpBMk1qWlVNRE0xT0RBMldnMEtWVWxFT2pFMk9EYzNNVEU0TmpFNQ0KTVRRdE9EWXlPVE5BYVdOaGJDNXRZWEoxWkc5MExtTnZiUTBLUkZSVFZFRlNWRHRVV2tsRVBVRnRaWEpwWTJFdlRHOXpYMEZ1WjJWcw0KWlhNNk1qQXlNekE0TVRGVU1UQXdNREF3RFFwRVZFVk9SRHRVV2tsRVBVRnRaWEpwWTJFdlRHOXpYMEZ1WjJWc1pYTTZNakF5TXpBNA0KTVROVU1UQXdNREF3RFFwVFZVMU5RVkpaT2tSRlJpQkRUMDRnUTFSR0lESXdNak1nUm1sdVlXeHpEUXBFUlZORFVrbFFWRWxQVGpvOA0KYUhSdGJENWNianhvWldGa1BseHVJQ0E4ZEdsMGJHVStVMk5vWldSMWJHVThMM1JwZEd4bFBseHVJQ0E4YzNSNWJHVStYRzRnSUNBZw0KZEdGaWJHVWdlMXh1SUNBZ0lDQWdZbTl5WkdWeUxXTnZiR3hoY0hObE9pQmpiMnhzWVhCelpWdzdYRzRnSUNBZ0lDQjNhV1IwYURvZw0KTVRBd0pWdzdYRzRnSUNBZ2ZWeHVJQ0FnSUZ4dUlDQWdJSFJvWEN3Z2RHUWdlMXh1SUNBZ0lDQWdkR1Y0ZEMxaGJHbG5iam9nYkdWbQ0KZEZ3N1hHNGdJQ0FnSUNCd1lXUmthVzVuT2lBNGNIaGNPMXh1SUNBZ0lIMWNiaUFnSUNCY2JpQWdJQ0IwYUNCN1hHNGdJQ0FnSUNCaQ0KWVdOclozSnZkVzVrTFdOdmJHOXlPaUFqWmpKbU1tWXlYRHRjYmlBZ0lDQjlYRzRnSUR3dmMzUjViR1UrWEc0OEwyaGxZV1ErWEc0OA0KWW05a2VUNWNiaUFnUEdneVBsTmphR1ZrZFd4bFBDOW9NajVjYmlBZ1BHSnlMejVjYmlBZ1BIUmhZbXhsUGx4dUlDQWdJRHgwY2o1Yw0KYmlBZ0lDQWdJRHgwYUQ1RVlYa2dNU0F0SUVGMVozVnpkQ0F4TVhSb1BDOTBhRDVjYmlBZ0lDQThMM1J5UGx4dUlDQWdJRHgwY2o1Yw0KYmlBZ0lDQWdJRHgwWkQ0NU9qQXdJRUZOSUMwZ01UQTZNREFnUVUwOEwzUmtQbHh1SUNBZ0lDQWdQSFJrUGxKbFoybHpkSEpoZEdsdg0KYmlCaGJtUWdRMmhsWTJzdGFXNDhMM1JrUGx4dUlDQWdJRHd2ZEhJK1hHNGdJQ0FnUEhSeVBseHVJQ0FnSUNBZ1BIUmtQakV3T2pBdw0KSUVGTklDMGdNVEU2TURBZ1FVMDhMM1JrUGx4dUlDQWdJQ0FnUEhSa1BrOXdaVzVwYm1jZ1EyVnlaVzF2Ym5rOEwzUmtQbHh1SUNBZw0KSUR3dmRISStYRzRnSUNBZ1BIUnlQbHh1SUNBZ0lDQWdQSFJrUGpFeE9qQXdJRUZOSUMwZ01Ub3dNQ0JRVFR3dmRHUStYRzRnSUNBZw0KSUNBOGRHUStTbVZ2Y0dGeVpIa2dVbTkxYm1ROEwzUmtQbHh1SUNBZ0lEd3ZkSEkrWEc0Z0lDQWdQSFJ5UGx4dUlDQWdJQ0FnUEhSaw0KUGpFNk1EQWdVRTBnTFNBME9qQXdJRkJOUEM5MFpENWNiaUFnSUNBZ0lEeDBaRDVCZEhSaFkyc2dZVzVrSUVSbFptVnVjMlU4TDNSaw0KUGx4dUlDQWdJRHd2ZEhJK1hHNGdJQ0FnUEhSeVBseHVJQ0FnSUNBZ1BIUmtQalE2TURBZ1VFMGdMU0ExT2pBd0lGQk5QQzkwWkQ1Yw0KYmlBZ0lDQWdJRHgwWkQ1TGFXNW5JRzltSUhSb1pTQklhV3hzSUNoTGIzUklLVHd2ZEdRK1hHNGdJQ0FnUEM5MGNqNWNiaUFnUEM5MA0KWVdKc1pUNWNiaUFnWEc0Z0lEeDBZV0pzWlQ1Y2JpQWdJQ0E4ZEhJK1hHNGdJQ0FnSUNBOGRHZytSR0Y1SURJZ0xTQkJkV2QxYzNRZw0KTVRKMGFEd3ZkR2crWEc0Z0lDQWdQQzkwY2o1Y2JpQWdJQ0E4ZEhJK1hHNGdJQ0FnSUNBOGRHUStPVG93TUNCQlRTQXRJREV3T2pBdw0KSUVGTlBDOTBaRDVjYmlBZ0lDQWdJRHgwWkQ1RGFHVmpheTFwYmp3dmRHUStYRzRnSUNBZ1BDOTBjajVjYmlBZ0lDQThkSEkrWEc0Zw0KSUNBZ0lDQThkR1ErTVRBNk1EQWdRVTBnTFNBeE9qQXdJRkJOUEM5MFpENWNiaUFnSUNBZ0lEeDBaRDVUWlcxcExVWnBibUZzY3lBbw0KU21WdmNHRnlaSGtnVW05MWJtUXBQQzkwWkQ1Y2JpQWdJQ0E4TDNSeVBseHVJQ0FnSUR4MGNqNWNiaUFnSUNBZ0lEeDBaRDR4T2pBdw0KSUZCTklDMGdNam93TUNCUVRUd3ZkR1ErWEc0Z0lDQWdJQ0E4ZEdRK1RIVnVZMmdnUW5KbFlXczhMM1JrUGx4dUlDQWdJRHd2ZEhJKw0KWEc0Z0lDQWdQSFJ5UGx4dUlDQWdJQ0FnUEhSa1BqSTZNREFnVUUwZ0xTQTJPakF3SUZCTlBDOTBaRDVjYmlBZ0lDQWdJRHgwWkQ1VA0KWlcxcExVWnBibUZzY3lBb1FYUjBZV05ySUdGdVpDQkVaV1psYm5ObEtUd3ZkR1ErWEc0Z0lDQWdQQzkwY2o1Y2JpQWdQQzkwWVdKcw0KWlQ1Y2JpQWdYRzRnSUR4MFlXSnNaVDVjYmlBZ0lDQThkSEkrWEc0Z0lDQWdJQ0E4ZEdnK1JHRjVJRE1nTFNCQmRXZDFjM1FnTVROMA0KYUR3dmRHZytYRzRnSUNBZ1BDOTBjajVjYmlBZ0lDQThkSEkrWEc0Z0lDQWdJQ0E4ZEdRK09Ub3dNQ0JCVFNBdElERXdPakF3SUVGTg0KUEM5MFpENWNiaUFnSUNBZ0lEeDBaRDVEYUdWamF5MXBiand2ZEdRK1hHNGdJQ0FnUEM5MGNqNWNiaUFnSUNBOGRISStYRzRnSUNBZw0KSUNBOGRHUStNVEE2TURBZ1FVMGdMU0F6T2pBd0lGQk5QQzkwWkQ1Y2JpQWdJQ0FnSUR4MFpENUhjbUZ1WkNCR2FXNWhiSE04TDNSaw0KUGx4dUlDQWdJRHd2ZEhJK1hHNGdJQ0FnUEhSeVBseHVJQ0FnSUNBZ1BIUmtQak02TURBZ1VFMGdMU0EwT2pBd0lGQk5QQzkwWkQ1Yw0KYmlBZ0lDQWdJRHgwWkQ1RGJHOXphVzVuSUVObGNtVnRiMjU1SUdGdVpDQkJkMkZ5WkhNZ1VISmxjMlZ1ZEdGMGFXOXVQQzkwWkQ1Yw0KYmlBZ0lDQThMM1J5UGx4dUlDQThMM1JoWW14bFBseHVJQ0E4WW5JdlBseHVJQ0JXYVhOcGRDQThZU0JvY21WbVBTSm9kSFJ3Y3pvdg0KTDNOMGIzSmhaMlV1WjI5dloyeGxZWEJwY3k1amIyMHZaR1ZtWTI5dUxXNWhkWFJwYkhWekwzWmxiblZsTFdkMWFXUmxMbWgwYld3aQ0KUGxabGJuVmxJRTFoY0hNOEwyRStJR2xtSUhsdmRTQmtiMjRuZENCM2FYTm9JSFJ2SUdkbGRDQnNiM04wTGlCWmIzVWdZMkZ1SUdGcw0KYzI4Z1BHRWdhSEpsWmowaWFIUjBjSE02THk5dVlYVjBhV3gxY3k1dmNtY3ZJajV6ZFdKelkzSnBZbVVnZEc4Z2IzVnlJRzVsZDNOcw0KWlhSMFpYSThMMkUrTGx4dUlDQThZbkl2UGp4aWNpOCtYRzRnSUR4cFBrNWhkWFJwYkhWeklFbHVjM1JwZEhWMFpUd3ZhVDVjYmp3dg0KWW05a2VUNWNiand2YUhSdGJENE5Da3hQUTBGVVNVOU9Pa05oWlhOaGNuTWdSbTl5ZFcwTkNrVk9SRHBXUlZaRlRsUU5Da1ZPUkRwVw0KUTBGTVJVNUVRVkk9)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/636a7879-7d13-4fb1-95ef-d8ef15036457)

Xem xét kĩ thông tin thu được , ở dưới cùng của đoạn mã sau khi decode có 2 đường link trỏ đến 1 web nào đó , sau khi kiểm tra cả 2 thì đường link đầu tiên là đáng chú ý , nó dẫn đến 1 trang web cho mình để tải 1 file `vbs` về :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/73334fed-6567-40ef-b4af-87977b17f659)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/9da8f62e-13e5-4576-be4b-a8133189eebf)

Có thể đọc thêm về file `vbs` ở đây : [link](https://vi.wikipedia.org/wiki/VBScript)

Tải file đó về và xem qua : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/630fb330-b885-4825-8ded-5c690f62df61)

Thông tin file khá là dài nhưng 1 số điểm cần lưu ý : đoạn mã cuối của file đã bị `obfuscate` nên có thể mình sẽ cần `deobfuscate` chúng đi để xem thông tin đó là gì 

Thứ 2 là đoạn mã chỗ này giống như đang lấy dữ liệu quan trọng lên server :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/f94d11e7-c70d-4d4e-a23d-9c4000c5a3be)

Dữ liệu cần gửi ở hàm này : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/48359650-b8d5-4d48-8950-c85a0dee4e88)

Và mình thử in chúng ra xem nó là gì ngay sau đó  :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/a6bff64c-2ed2-4012-a58d-7f0aab96a063)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/1614b999-564c-4eb7-9a90-900163a956b0)

Thu được 1 đường [link](https://download1647.mediafire.com/l188u2d532qg3fOoLpilcI89p0_h4E0cGLjk_uvBUiag7E_rMZ-H5-me9Kr9SQLVQaKSiKcEvJO-EkfTSUqWlrN6SzXgI0LYBh-F5em4IA4iX3tOIGh0Ej46GlwvLOfT8pzvuy91Utej1r2I0jg7YsUNcssPted508dskWRpkAI/yea535hvgp32vmv/defcon-flag.png.XORed) để download file `Flag` nhưng đã bị encrypt, có vẻ như lại là thuật toán `Xor` tiếp , vậy sẽ cần `Key` nữa để `xor` với thông tin trong file 

Còn 1 Đoạn dữ liệu mà mình chưa đụng đến đó là đoạn mã cuối đã bị `obfuscate` hãy thử tìm kiếm về `deobfuscate` của file `vbs` và mình tìm thấy 1 [Github](https://github.com/DoctorLai/VBScript_Obfuscator/blob/master/vbs_defuscator.vbs) giúp cho việc đó

Copy đoạn mã đó sang 1 file lưu dưới .vbs khác và thực hiện `deobfuscate` nó :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/a873a12f-0fae-4ea0-8882-0f558b56f2ca)

Đây là đoạn mã gửi yêu cầu lên server và đáng chú ý là `strUser` ở đây không ghi cụ thể là ai nên trước tiên mình thử với tên bất kì thu được kết quả là `Not Admin` :

Để làm được điều đó hãy Sửa `strUser` với tên bất kì và chạy chúng sau đó mở `wireshark` lên và bắt gói tin  để xem chi tiết :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/9476e321-8e9f-48a7-85ec-95457784170f)

Thông báo không phải là Admin nên mình có thể sửa lại với User Admin và gửi lên lần nữa :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/657a4b02-61b4-47e5-bda8-df84f41f9b8c)

Thông báo cho thấy ta thu được `Key` ở đây , Đem nó đi `XOR` với file `Flag` vừa thu được khi nãy và thu được 1 bức ảnh chứa `flag` cần tìm :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/ec820a13-5690-4151-a425-9136c394fc7c)

Ở đây có thể không thấy gì , chú ý thì 1 bài byte của file nói đây là file `PNG` vì vậy hãy ấn vào `magic wand` của cyberchef nó sẽ giúp hiển thị ra hình ảnh, khá là hữu dụng :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/f0bce9bb-f29f-40d7-8768-192130523463)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/8c096a9e-eb6d-4bb9-8ac3-ab9367748476)

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/078dad11-4b82-44cd-a36c-ca70f344ef7d)

`Flag` : `SEKAI{so_i_guess_we'll_get_more_better_next_year-_-}`











