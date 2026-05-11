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
> {: .highlight }  
>  1.00	  2.00	  3.00	  4.00	  5.00	  
>  6.00	  7.00	  8.00	  9.00	 10.00	


이번에는 행과 열을 바꿔서 파일에 적어봅시다. `i`와 `j`을 적절히 바꿔주면 됩니다. 이중 for 문을 이해하기 위한 가장 좋은 예시이자, 가장 많이 활용되니 확실하게 이해해둡시다!


### **🚀 TRY IT!**
{: .text-blue-200 }

```python
row, col = np.shape(data)

f = open("array2.txt", 'w')

for j in range(col):
    for i in range(row):
        f.write(f'{data[j][i]:6.2f}\t') # \t으로 띄어쓰기
    f.write('\n') # 한 행이 다 끝나면 줄 바꿈
f.close()
```

## **`np.savetxt()`를 이용한 저장**
NumPy의 `np.savetxt()` 함수를 사용하면 반복문을 사용하여 데이터를 하나씩 쓸 필요 없이, 배열 전체를 한 번에 구조화된 파일로 내보낼 수 있습니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
# np.savetxt(파일명, 데이터, 형식, 구분자, 헤더) 
np.savetxt('array3.txt', data, fmt='%6.2f')

# 행과 열 바꿔 저장
dataT=data.transpose() 
np.savetxt('array3.txt', dataT, fmt='%6.2f')
```

## **🛠️ `np.savetxt()` 함수의 기본 형식**

`np.savetxt()`는 다음과 같은 기본적인 인자들을 활용할 수 있습니다.

```python
np.savetxt('filename', data, fmt='', delimiter='', header='')
```

예시를 통해 각 인자의 역할을 알아봅시다.

```python
np.savetxt('test.csv', data, fmt = '%.2f', delimiter=',', header = 'c1, c2, c3, c4, c5')
```

- `'test.csv'` (filename): 저장하고자 하는 파일의 이름 (혹은 경로)입니다.
- `data` : 파일로 저장할 실제 NunPy 배열 데이터입니다.
- `fmt = '%.2f'` (format): 파일에 적히는 숫자 형식을 결정합니다.
- `delimiter=','` (구분자): 데이터 값 사이를 구분할 문자를 지정합니다. 기본값은 공백이며, 예시처럼 콤마 (`,`)를 사용하면 엑셀에서 바로 열 수 있는 csv 형식으로 저장할 수 있습니다.
- `header = '...'` (헤더): 파일의 맨 첫 줄에 들어갈 열 이름(제목)을 지정합니다. 데이터가 각각 무엇을 의미하는지 설명할 때 사용합니다. 맨 앞에 **#**이 있으면 컴퓨터는 데이터로 생각하지 않고 주석처리 된 글로 인지합니다.