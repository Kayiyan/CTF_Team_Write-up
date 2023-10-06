
# Needle in the Wifi Stack

### _Points: 100_

### _Description:_

[frames.pcap](https://github.com/shmily-2010/CTF_Writeups/blob/55f9bbf76462275cd90b104d1e283a4cf5f04c7a/BuckeyeCTF/Misc/Needle%20in%20the%20Wifi%20Stack/frames.pcap)

### _Author: arcsolstice_

### _Solution:_

Khi mở file pcap, mình thấy rất nhiều traffic và mỗi traffic đều có SSID là một mã base 64. 

![image](https://github.com/shmily-2010/CTF_Writeups/assets/112896213/90ee1272-6789-494d-9804-6c824044a30d)

Mình nghi ngờ rằng mã base 64 này sau khi decode sẽ thu được flag. Đồng thời, mình thấy các traffic cũng bị lặp lại. Nên quá trình lọc các mã base64 của mình sẽ là lọc ra base 64, lọc các mã bị lặp, loại bỏ những kí tự thừa và cuối cùng là encode từng dòng. 

Lọc các mã base 64
```Python
# Mở tệp gốc để đọc
with open('pcapData.txt', 'r') as file:
    lines = file.readlines()

# Mở tệp để ghi, sử dụng chế độ 'w' để ghi đè nội dung cũ
with open('filterData.txt', 'w') as new_file:
    inside_quotes = False  # Sử dụng để theo dõi xem chúng ta có đang ở trong dòng chứa """ hay không
    for line in lines:
        # Kiểm tra nếu dòng bắt đầu bằng """ và chưa nằm trong """ trước đó
        if line.startswith('"""') and not inside_quotes:
            inside_quotes = True
        # Nếu đang trong """ và dòng hiện tại cũng bắt đầu bằng """ thì thoát khỏi trạng thái """
        elif inside_quotes and line.startswith('"""'):
            inside_quotes = False
        # Nếu không ở trong trạng thái """, thêm "   " vào đầu và cuối dòng
        elif not inside_quotes:
            line = '   ' + line.strip() + '   \n'
            # Ghi dòng đã được xử lý vào tệp mới
            new_file.write(line)
```

Lọc các mã bị lặp
```Python
# Mở tệp gốc để đọc
with open('filterData.txt', 'r') as file:
    lines = file.readlines()

# Mở tệp để ghi, sử dụng chế độ 'w' để ghi đè nội dung cũ
with open('clean.txt', 'w') as new_file:
    # Tạo một danh sách để theo dõi các dòng đã xuất hiện
    seen_lines = set()
    for line in lines:
        # Xóa khoảng trắng ở đầu và cuối dòng và kiểm tra xem dòng đã tồn tại chưa
        cleaned_line = line.strip()
        if cleaned_line not in seen_lines:
            # Ghi dòng vào tệp mới nếu nó chưa tồn tại và thêm nó vào danh sách
            new_file.write(line)
            seen_lines.add(cleaned_line)
```

Decode thành flag
```Python
import base64

def decode_and_write(data, output_file):
    try:
        decoded_data = base64.b64decode(data)
        decoded_str = decoded_data.decode('utf-8')
        with open(output_file, 'a') as flag_file:
            flag_file.write(decoded_str + '\n')
    except Exception as e:
        print(f"Decode failed: {e}")

def main(input_file, output_file):
    with open(input_file, 'r') as file:
        for line in file:
            if len(line) > 3:
                line = line.rstrip('0')  # Bỏ kí tự 0 ở cuối dòng
                decode_and_write(line, output_file)
            else:
                if len(line) > 0:
                    line = line[1:]  # Xóa kí tự đầu tiên
                    decode_and_write(line, output_file)

if __name__ == "__main__":
    input_file = "clean.txt"  # Thay đổi thành tên tệp đầu vào của bạn
    output_file = "flag.txt"  # Tên tệp đầu ra
    main(input_file, output_file)
```

Đọc file flag.txt là ta sẽ thu được flag.

> **Flag**: `bctf{tw0_po1nt_4_g33_c0ng3s7i0n}`
