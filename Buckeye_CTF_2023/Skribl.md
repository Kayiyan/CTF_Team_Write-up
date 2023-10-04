# Name : Skribl
# Description : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/cbbeaec8-23e2-49b5-bb6f-89baee391190)

# Solution 

 Docker file : 

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/28b5d0fa-87e5-4b53-bb98-2990dab2e0a7)

Có thể thấy `Flag` được đặt thành biến môi trường, tuy nhiên, nó không được sử dụng trong bất kỳ mã Python nào.

File `skribl.py` :

```python
import math
import time
from flask import Flask, render_template, redirect, url_for, request
from flask_bootstrap import Bootstrap5

from flask_wtf import FlaskForm, CSRFProtect
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired, Length

# Don't try this at home, kids
try:
    from backend import create_skribl, init_backend
except:
    from .backend import create_skribl, init_backend

app = Flask(__name__)
app.secret_key = 'tO$&!|0wkamvVia0?n$NqIRVWOG'

bootstrap = Bootstrap5(app)
csrf = CSRFProtect(app)

skribls = {}
stime = math.floor(time.time())

init_backend(skribls)

class SkriblForm(FlaskForm):
    skribl = StringField('Your message: ', validators=[DataRequired(), Length(1, 250)])
    author = StringField("Your name:", validators=[Length(0, 40)])
    submit = SubmitField('Submit')
	

@app.route('/', methods=['GET', 'POST'])
def index():
    form = SkriblForm()
    message = ""
    if form.validate_on_submit():
        message = form.skribl.data
        author = form.author.data

        key = create_skribl(skribls, message, author)
        return redirect(url_for('view', key=key))
    
    return render_template('index.html', form=form, error_msg=request.args.get("error_msg", ''))

@app.route('/view/<key>', methods=['GET'])
def view(key):
    print(f"Viewing with key {key}")
    if key in skribls:
        message, author = skribls[key]
        return render_template("view.html", message=message, author=author, key=key)
    else:
        return redirect(url_for('index', error_msg=f"Skribl not found: {key}"))
    
@app.route('/about', methods=["GET"])
def about():
    return render_template('about.html')


@app.context_processor
def inject_stime():
    return dict(stime=math.floor(time.time()) - stime)
```
Một số chú ý vể file : 

`/ (root route)`:Xử lý cả yêu cầu GET và POST. Nó hiển thị một mẫu HTML có tên `index.html` và xử lý các yêu cầu gửi biểu mẫu. Nếu biểu mẫu được gửi thành công, nó chuyển hướng đến `view`.

`/view/<key>`: Xử lý các yêu cầu GET để xem một `skribl` cụ thể bằng một khóa duy nhất. Nó kiểm tra xem khóa có tồn tại trong từ điển skribls và hiển thị skribl hoặc chuyển hướng đến root với thông báo lỗi.

`/about`: Xử lý các yêu cầu GET để trang `about`.

Có thể thấy nếu ta tìm được key đúng thì sẽ giải quyết được bài này nhưng ở đây không có liên quan gì đến key , hãy tìm kiếm các file khác trong các thư mục còn lại:

Trong thư mục `chal/_pycache_` có chứa file `backend` có thể sẽ chứa thông tin quan trong nhưng nó đã được complie , ở đây file complie bằng python 3.13 nên ta không thể uncompy chúng bằng các công cụ bình thường vì chưa support cho python 3.13, nên sẽ cần tải python 3.13 từ source dev.

Tải souce cpython về và cài đặt python 3.13  [link](https://github.com/python/cpython)

Tools cho decomplie pyc 3.13 : [link](https://github.com/nedbat/coveragepy/blob/coverage-5.6b1/lab/show_pyc.py)

Không cần sài hết công cụ , chỉ cần file show_pyc.py để decomplie file backend :

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/126185640/cce726a3-c0b6-4d2e-b683-b28ded47e7ce)

Full code decomplie của file `backend` :

```python

magic b'eb0d0d0a'
flags 0x00000000
moddate b'495c1865' (Sat Sep 30 13:35:05 2023)
pysize b'f6010000' (502)
code
    name '<module>'
    argcount 0
    nlocals 0
    stacksize 2
    flags 0000: 
    code
       9500530053014b007200530053014b017201530053014b02720253005301
       4b037203530053014b04720453025c05340253031a006a04720653041a00
       72076701
  0           0 RESUME                   0

  1           2 LOAD_CONST               0 (0)
              4 LOAD_CONST               1 (None)
              6 IMPORT_NAME              0 (string)
              8 STORE_NAME               0 (string)

  2          10 LOAD_CONST               0 (0)
             12 LOAD_CONST               1 (None)
             14 IMPORT_NAME              1 (random)
             16 STORE_NAME               1 (random)

  3          18 LOAD_CONST               0 (0)
             20 LOAD_CONST               1 (None)
             22 IMPORT_NAME              2 (time)
             24 STORE_NAME               2 (time)

  4          26 LOAD_CONST               0 (0)
             28 LOAD_CONST               1 (None)
             30 IMPORT_NAME              3 (math)
             32 STORE_NAME               3 (math)

  5          34 LOAD_CONST               0 (0)
             36 LOAD_CONST               1 (None)
             38 IMPORT_NAME              4 (os)
             40 STORE_NAME               4 (os)

  8          42 LOAD_CONST               2 ('return')
             44 LOAD_NAME                5 (str)
             46 BUILD_TUPLE              2
             48 LOAD_CONST               3 (<code object create_skribl at 0x7f87d6ffe3d0, file "/home/rene/Documents/Java/OSUCyberSecurityClub/repos/rctf-terraform/buckeyectf-hosting/buckeyectf-challenges/chals/rev-pycache/dist/chal/backend.py", line 8>)
             50 MAKE_FUNCTION
             52 SET_FUNCTION_ATTRIBUTE   4 (annotations)
             54 STORE_NAME               6 (create_skribl)

 18          56 LOAD_CONST               4 (<code object init_backend at 0x7f87d6b368d0, file "/home/rene/Documents/Java/OSUCyberSecurityClub/repos/rctf-terraform/buckeyectf-hosting/buckeyectf-challenges/chals/rev-pycache/dist/chal/backend.py", line 18>)
             58 MAKE_FUNCTION
             60 STORE_NAME               7 (init_backend)
             62 RETURN_CONST             1 (None)
    consts
        0: 0
        1: None
        2: 'return'
        3: code
            name 'create_skribl'
            argcount 3
            nlocals 7
            stacksize 6
            flags 0003: CO_OPTIMIZED, CO_NEWLOCALS
            code
               95005b010000000000000000530155010e00330235010000000000002000
               5b0200000000000000005204000000000000000000000000000000000000
               5b0200000000000000005206000000000000000000000000000000000000
               2d0000005b02000000000000000052080000000000000000000000000000
               000000002d0000006e035b0b000000000000000053023501000000000000
               1300560473022f007302481900006e045b0c0000000000000000520e0000
               000000000000000000000000000000002200550335010000000000005002
               4d1b00000b006e056e045303521100000000000000000000000000000000
               0000550535010000000000006e0658123402580627000000550624007302
               200073026e046600
   8           0 RESUME                   0

   9           2 LOAD_GLOBAL              1 (print + NULL)
              12 LOAD_CONST               1 ('Creating skribl ')
              14 LOAD_FAST                1 (message)
              16 FORMAT_SIMPLE
              18 BUILD_STRING             2
              20 CALL                     1
              28 POP_TOP

  11          30 LOAD_GLOBAL              2 (string)
              40 LOAD_ATTR                4 (ascii_lowercase)
              60 LOAD_GLOBAL              2 (string)
              70 LOAD_ATTR                6 (ascii_uppercase)
              90 BINARY_OP                0 (+)
              94 LOAD_GLOBAL              2 (string)
             104 LOAD_ATTR                8 (digits)
             124 BINARY_OP                0 (+)
             128 STORE_FAST               3 (alphabet)

  12         130 LOAD_GLOBAL             11 (range + NULL)
             140 LOAD_CONST               2 (40)
             142 CALL                     1
             150 GET_ITER
             152 LOAD_FAST_AND_CLEAR      4 (i)
             154 SWAP                     2
             156 BUILD_LIST               0
             158 SWAP                     2
         >>  160 FOR_ITER                25 (to 214)
             164 STORE_FAST               4 (i)
             166 LOAD_GLOBAL             12 (random)
             176 LOAD_ATTR               14 (choice)
             196 PUSH_NULL
             198 LOAD_FAST                3 (alphabet)
             200 CALL                     1
             208 LIST_APPEND              2
             210 JUMP_BACKWARD           27 (to 160)
         >>  214 END_FOR
             216 STORE_FAST               5 (key_list)
             218 STORE_FAST               4 (i)

  14         220 LOAD_CONST               3 ('')
             222 LOAD_ATTR               17 (join + NULL|self)
             242 LOAD_FAST                5 (key_list)
             244 CALL                     1
             252 STORE_FAST               6 (key)

  15         254 LOAD_FAST_LOAD_FAST     18 (message, author)
             256 BUILD_TUPLE              2
             258 LOAD_FAST_LOAD_FAST      6 (skribls, key)
             260 STORE_SUBSCR

  16         264 LOAD_FAST                6 (key)
             266 RETURN_VALUE

None     >>  268 SWAP                     2
             270 POP_TOP

  12         272 SWAP                     2
             274 STORE_FAST               4 (i)
             276 RERAISE                  0
ExceptionTable:
  156 to 214 -> 268 [2]
            consts
                0: None
                1: 'Creating skribl '
                2: 40
                3: ''
            names ('print', 'string', 'ascii_lowercase', 'ascii_uppercase', 'digits', 'range', 'random', 'choice', 'join')
            varnames ('skribls', 'message', 'author', 'alphabet', 'i', 'key_list', 'key')
            freevars ()
            cellvars ()
            filename '/home/rene/Documents/Java/OSUCyberSecurityClub/repos/rctf-terraform/buckeyectf-hosting/buckeyectf-challenges/chals/rev-pycache/dist/chal/backend.py'
            firstlineno 8
/home/kali/Downloads/rev/buckeye/Skribl/coveragepy/lab/show_pyc.py:116: DeprecationWarning: co_lnotab is deprecated, use co_lines instead.
  show_hex("lnotab", code.co_lnotab, indent=indent)
            lnotab 02011c0264015a0222010a0108fc
/home/kali/Downloads/rev/buckeye/Skribl/coveragepy/lab/show_pyc.py:145: DeprecationWarning: co_lnotab is deprecated, use co_lines instead.
  byte_increments = bytes_to_ints(code.co_lnotab[0::2])
/home/kali/Downloads/rev/buckeye/Skribl/coveragepy/lab/show_pyc.py:146: DeprecationWarning: co_lnotab is deprecated, use co_lines instead.
  line_increments = bytes_to_ints(code.co_lnotab[1::2])
                8:0, 9:2, 11:30, 12:130, 14:220, 15:254, 16:264, 12:272
            linetable
               8000dc0409d00c1c98579849d00a26d40427e40f15d70f25d10f25ac06d7
               283ed1283ed10f3ec416c71dc11dd10f4e8048dc3136b072b319d60f3ba8
               419406970d920d9868d61027d00f3b8048d00f3be00a0c8f2789279028d3
               0a1b8043d8141bd013248047814cd80b0e804af9f20900103c
                co_lines 8:0-2, 9:2-30, 11:30-130, 12:130-220, 14:220-254, 15:254-264, 16:264-268, None:268-272, 12:272-278
        4: code
            name 'init_backend'
            argcount 1
            nlocals 1
            stacksize 6
            flags 0003: CO_OPTIMIZED, CO_NEWLOCALS
            code
               95005b000000000000000000520200000000000000000000000000000000
               000022005b04000000000000000052060000000000000000000000000000
               0000000022005b0800000000000000005208000000000000000000000000
               000000000000220035000000000000003501000000000000350100000000
               000020005b0b000000000000000055005b0c0000000000000000520e0000
               000000000000000000000000000000005301050000005302350300000000
               000020006700
 18           0 RESUME                   0

 19           2 LOAD_GLOBAL              0 (random)
             12 LOAD_ATTR                2 (seed)
             32 PUSH_NULL
             34 LOAD_GLOBAL              4 (math)
             44 LOAD_ATTR                6 (floor)
             64 PUSH_NULL
             66 LOAD_GLOBAL              8 (time)
             76 LOAD_ATTR                8 (time)
             96 PUSH_NULL
             98 CALL                     0
            106 CALL                     1
            114 CALL                     1
            122 POP_TOP

 21         124 LOAD_GLOBAL             11 (create_skribl + NULL)
            134 LOAD_FAST                0 (skribls)
            136 LOAD_GLOBAL             12 (os)
            146 LOAD_ATTR               14 (environ)
            166 LOAD_CONST               1 ('FLAG')
            168 BINARY_SUBSCR
            172 LOAD_CONST               2 ('rene')
            174 CALL                     3
            182 POP_TOP
            184 RETURN_CONST             0 (None)
            consts
                0: None
                1: 'FLAG'
                2: 'rene'
            names ('random', 'seed', 'math', 'floor', 'time', 'create_skribl', 'os', 'environ')
            varnames ('skribls',)
            freevars ()
            cellvars ()
            filename '/home/rene/Documents/Java/OSUCyberSecurityClub/repos/rctf-terraform/buckeyectf-hosting/buckeyectf-challenges/chals/rev-pycache/dist/chal/backend.py'
            firstlineno 18
            lnotab 02017a02
                18:0, 19:2, 21:124
            linetable
               8000dc040a874b824b9404970a920a9c349f399a399b3bd31027d40428e4
               041190279c329f3a993aa066d11b2da876d50436
                co_lines 18:0-2, 19:2-124, 21:124-186
    names ('string', 'random', 'time', 'math', 'os', 'str', 'create_skribl', 'init_backend')
    varnames ()
    freevars ()
    cellvars ()
    filename '/home/rene/Documents/Java/OSUCyberSecurityClub/repos/rctf-terraform/buckeyectf-hosting/buckeyectf-challenges/chals/rev-pycache/dist/chal/backend.py'
    firstlineno 1
    lnotab 00ff0201080108010801080108030e0a
        0:0, 1:2, 2:10, 3:18, 4:26, 5:34, 8:42, 18:56
    linetable
       f003010101db000ddb000ddb000bdb000bdb0009f00608010fa873f40008
       010ff314030137
        co_lines 0:0-2, 1:2-10, 2:10-18, 3:18-26, 4:26-34, 5:34-42, 8:42-56, 18:56-64

```

Tóm tắt lại về py :

```python

# Tên module: '<module>'
# Số lượng đối số: 0
# Số lượng biến cục bộ: 0
# Kích thước ngăn xếp: 2
# Flag: 0000

# Mã chương trình
# Mã này bao gồm nhiều dòng số và hàm đã định nghĩa trong tệp backend.py
# Dưới đây là một phần của mã đã được giải mã:

# Hàm create_skribl
def create_skribl(skribls, message, author):
    print('Creating skribl', message, author)

    import string
    import random

    alphabet = string.ascii_lowercase + string.ascii_uppercase + string.digits

    key_list = [random.choice(alphabet) for i in range(40)]
    key = ''.join(key_list)

    skribls[message, author] = key
    return key

# Hàm init_backend
def init_backend(skribls):
    import random
    import math
    import time

    random.seed(int(math.floor(time.time())))

    create_skribl(skribls, os.environ['FLAG'], 'rene')

# Dưới đây là một số thông tin về mã:
# - Mã chương trình đã được tạo ra bằng Python và sau đó đã được biên dịch thành bytecode.
# - Mã chương trình này đã được giải mã thành mã nguồn Python gần đúng nhưng không hoàn chỉnh.
# - Có hai hàm chính trong mã: create_skribl và init_backend, chúng sẽ được gọi khi cần.

```
Mã này định nghĩa hai hàm chính `create_skribl` và `init_backend`, dưới đây là giải thích về mỗi hàm:

1. **create_skribl**: Hàm này dùng để tạo một skribl mới và lưu trữ nó vào `skribls` - một từ điển được truyền vào hàm. Skribl có thể hiểu là một thông điệp với một tên tác giả liên quan. Dưới đây là cách nó hoạt động:

   - In ra thông báo "Creating skribl" kèm theo nội dung thông điệp (`message`) và tên tác giả (`author`).
   - Import thư viện `string` và `random`.
   - Tạo một biến `alphabet` chứa tất cả các ký tự chữ cái thường, chữ cái hoa và chữ số.
   - Tạo một danh sách `key_list` gồm 40 ký tự được chọn ngẫu nhiên từ `alphabet`.
   - Kết hợp các ký tự trong `key_list` thành một chuỗi `key`.
   - Lưu thông điệp và tên tác giả kèm theo `key` vào từ điển `skribls` bằng cách sử dụng cặp giá trị `(message, author)` làm khóa.
   - Cuối cùng, trả về giá trị `key` làm kết quả của hàm.

2. **init_backend**: Hàm này được gọi để khởi tạo ứng dụng backend. Dưới đây là cách nó hoạt động:

   - Import các thư viện `random`, `math`, và `time`.
   - Thiết lập seed cho trình tạo số ngẫu nhiên `random` dựa trên thời gian hiện tại, đảm bảo rằng các số ngẫu nhiên sẽ thay đổi theo thời gian.
   - Gọi hàm `create_skribl` để tạo skribl với nội dung từ biến môi trường `os.environ['FLAG']` và tên tác giả 'rene'. Skribl này sẽ được lưu vào từ điển `skribls`.

Hàm `create_skribl` làm nhiệm vụ tạo skribl và lưu trữ thông điệp và tên tác giả vào từ điển `skribls`. Hàm `init_backend` được sử dụng để khởi tạo ứng dụng backend và tạo skribl đầu tiên.

Từ đây có thể thấy được cách web tạo seed, và dựa vào đó tính toán thời gian tạo ra và tạo khóa có độ dài là bằng nhau là được :
 
Mã khai thác :

```python
import math
import re
import random
import string
from datetime import datetime, timezone
import requests

def generate_seed():
    resp = requests.get("https://skribl.chall.pwnoh.io/")
    duration = int(re.search(r"stime = moment.duration\((\d+), 'seconds'\)", resp.text).group(1))
    return math.floor((datetime.now().timestamp() - duration))

def create_key(seed):
    random.seed(seed)
    alphabet = string.ascii_letters + string.digits
    return "".join(random.choice(alphabet) for _ in range(40))

def main():
    created_datetime = generate_seed()
    for i in range(-1, 2):
        key = create_key(created_datetime + i)
        print(f"Trying key: {key}")
        resp = requests.get(f"https://skribl.chall.pwnoh.io/view/{key}")
        if "bctf{" in resp.text:
            flag = re.search(r"bctf{.*}", resp.text).group()
            print("Flag found:", flag)
            return
    print("Flag not found")

if __name__ == "__main__":
    main()
```
`Flag` : `bctf{wHy_d0_w3_Ne3d_s0_m@ny_N0T3$_aNyW@y}`
