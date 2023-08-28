# Wiki game 

### _Points: 100_

### _Description:_

Welcome to Project SEKAI Online Judge!

> **Note:**
> 
> Time limit is 5 seconds. There is rate limiting and you can only submit once every 30 seconds. For simplicity, sample input 1.in is given in this challenge.

> [http://algo-server.chals.sekai.team](http://algo-server.chals.sekai.team)

[Wiki_Game.pdf](https://github.com/Kayiyan/CTF_Team_Write-up/blob/main/SekaiCTF%202023/PPC/Wiki%20Game/Wiki_Game.pdf) [1.in](https://github.com/Kayiyan/CTF_Team_Write-up/blob/main/SekaiCTF%202023/PPC/Wiki%20Game/1.in) [testcase.txt](https://github.com/Kayiyan/CTF_Team_Write-up/blob/main/SekaiCTF%202023/PPC/Wiki%20Game/testcase.txt)

### _Author: sahuang_

### _Solution:_

**Tóm tắt đề bài**

+ _Input:_
  - T : số testcases (1<=T<=20)
  - Trong mỗi test cases chúng ta sẽ có một đồ thị có hướng G(V, E) - V đỉnh, E cạnh - và hai đỉnh đầu (u) và đích (v).
+ _Output:_
  - T dòng
  - Mỗi dòng là kết quả của việc kiểm tra một test case xem có tồn tại một đường đi <=6 từ đỉnh u đến đỉnh v hay không.
  - Nếu có, in ra màn hình _YES_, không thì in ra _NO_

**Hướng giải quyết**

Chúng ta có thể sử dụng cả 2 thuật toán đối với bài này là DFS (Depth First Search) hoặc BFS (Breath First Search). Tuy nhiên việc sử dụng DFS sẽ lâu hơn BFS trong trường hợp trường hợp tồi tệ (worst case) hoặc có thể phải duyệt lại những đỉnh mà nó đã duyệt qua. Nhưng trong trường hợp tốt (bestcase) thì DFS có thể sẽ xử lý nhanh hơn. 

* Đối với BFS, chúng ta sẽ duyệt qua tất cả các đỉnh liền kề của các đỉnh bắt đầu từ root (theo thứ tự level, u = root = 0) khi level <=6. Nếu như có thể gặp được đỉnh v trong quá trình duyệt thì sẽ tồn tại một đường đi có độ dài <=6 từ đỉnh u đến đỉnh v

* Đối với DFS, chúng ta sẽ duyệt qua từng nhánh sâu nhất tính từ root (u = root = 0), nếu như level > 6 thì sẽ quay ngược lại node cha để kiểm tra các nhánh còn lại. Nếu như level <=6 mà gặp được v thì tồn tại một đường đi có độ dài <-6 từ đỉnh u đến đỉnh v

```python
def checkPath(srcVer, dstVer, length): 
    if length > 6:
        return False
    if srcVer == dstVer:
        return True
    
    visited[srcVer] = True

    for neighbor in graph[srcVer]:
        if not visited[neighbor]:
            if checkPath(neighbor, dstVer, length+1):
                return True
            
    visited[srcVer] = False

    return False

testcaseNum = int(input(),10)

for case in range(testcaseNum):
    verticesNum, edgesNum = map(int, input().split())

    graph = [[] for i in range(verticesNum)]
    visited = [False] * verticesNum

    for edges in range(edgesNum):
        startVer, endVer = map(int, input().split())
        graph[startVer].append(endVer)

    srcVer, dstVer = map(int, input().split())

    if checkPath(srcVer, dstVer, 0):
        print("YES")
    else:
        print("NO")
```
