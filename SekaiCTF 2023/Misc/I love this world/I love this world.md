# I love this world

### _Points: 100_

### _Description:_

Vocaloid is a great software to get your computer sing the flag out to you, but what if you can’t afford it? No worries, there are plenty of other free tools you can use. How about — let’s say — this one?

Flag format (Regex): SEKAI\{[A-Z0-9]+\}.

> **Note:**
> 
> No romanization or Japanese translation is needed to solve the challenge. The flag you find will satisfy the flag regex. The flag in Japanese is a fake flag.

[ilovethisworld.svp](https://github.com/Kayiyan/CTF_Team_Write-up/blob/af74eb0d1c18e84dada4531c037eae8eaafd0fec/SekaiCTF%202023/Misc/I%20love%20this%20world/ilovethisworld.svp)

### _Author: pamLELcu_

### _Solution:_

Trước khi đi phân tích bài thì mình thử tìm hiểu [file svp](https://fileinfo.com/extension/svp) là gì, để có thể hiểu và dễ dàng cho việc phân tích hơn. Và tôi biết được đây là một project của Synthesizer V Studio, nó được lưu dưới dạng JSON để tải các âm thanh của project. 

Mình đã thử mở bằng file bằng Synthesizer V Studio và VSCode, và nhận thấy có một flag `SEKAI{がぼくわすなんだ}` và đây là fake flag vì nó không thỏa mãn flag format.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/415dbf55-2edd-4821-abb3-b63617b27528)

Mặt khác, mình nhận ra các phonemes là phiên âm của các từ trong notes (thuật ngữ của Synthersizer V Studio) hay phần lyrics (trong file mở bằng VSCode).

Vì vậy, mình thử đọc phiên lại các phonemes ấy, mình sử dụng cat để lọc các phonemes một cách dễ dàng.

![image](https://github.com/Kayiyan/CTF_Team_Write-up/assets/112896213/599ce887-541a-4c5e-bdbc-b481236de85e)

`eh f` : F
`eh l` : L
`ey`   : A
`jh`   : G
`iy`   : is
`k ow l ax n`: dấu ':'
`eh s` : S
`iy`   : E
`k ey` : K
`ay`   : A
`ey`   : I
`ow p ax n k er l iy b r ae k ih t`: dấu '{'

Phần còn lại bạn hãy đoán nốt nhé ^^

> Flag: `SEKAI{some1zfarawaytmr15sequeltoourdreamtdy}`
