---
layout: default
title: 수치 데이터와 NumPy 배열
parent: Ch5. 파일 입출력
nav_order: 2
---

# **수치 데이터와 NumPy 배열**

---

단순한 텍스트를 넘어, 공학 계산에서 사용하는 숫자 배열(Array)을 다루는 방법을 배워봅시다. 수천 개의 데이터를 일일이 `write()`로 적는 대신, NumPy 라이브러리를 활용하면 단 한 줄의 코드로 데이터를 읽고 저장할 수 있습니다.

---

## **반복문을 이용한 저장**
가장 기본적인 방법은 이중 루프를 돌며 데이터를 하나씩 적는 것입니다.

다음과 같은 2차원 데이터를 가지고 시작해봅시다.

```python
import numpy as np

data = np.array([
    [1, 2, 3, 4 ,5], 
    [6, 7, 8, 9 ,10],
])

print(np.shape(data)) # (2,5) - row 2, column 5 
```

이중 for문과 write()을 활용하면 요소 하나하나를 파일에 적을 수 있습니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
row, col = np.shape(data)

f = open("array.txt", 'w')

for i in range(row):
    for j in range(col):
        f.write(f'{data[i][j]:6.2f}\t') # \t으로 띄어쓰기
    f.write('\n') # 한 행이 다 끝나면 줄 바꿈
f.close()
```
