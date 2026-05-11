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
---

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

---

## **CSV 파일로 저장 (엑셀)**

위에서 배운대로 파일 형식 (csv)와 구분자 (delimeter)를 활용하면 쉽게 엑셀에서 열 수 있는 형식으로 데이터를 저장할 수 있습니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
import numpy as np
data = np.random.random((100,5))
np.savetxt('test.csv', data, fmt = '%.2f', delimiter=',', header = 'c1, c2, c3, c4, c5')
```

`np.savetxt()` 같은 함수가 없더라도, 지난 시간에 배운 `write()` 함수와 `for` 문만으로도 충분히 CSV 파일을 만들 수 있습니다. CSV는 결국 **콤마로 구분된 텍스트**일 뿐이기 때문입니다. 다만 과정이 번거로워질 뿐입니다.

```python
import numpy as np
data = np.random.random((100,5))
with open("test.csv", "w") as f:
    # 1. 헤더(컬럼 이름) 작성
    f.write("c1,c2,c3,c4,c5\n")
    # 2. 데이터 한 줄씩 꺼내오기 (100번 반복)
    for row in data:
        line = f"{row[0]:.2f},{row[1]:.2f},{row[2]:.2f},{row[3]:.2f},{row[4]:.2f}\n"
        
        f.write(line)
```

---

 **`np.loadtxt()`를 이용한 데이터 불러오기**
데이터를 저장하는 것만큼 중요한 것이 저장된 데이터를 다시 불러오는 것입니다. NumPy의 `np.loadtxt()` 함수를 사용하면 텍스트 파일이나 CSV 파일에 저장된 수치 데이터를 즉시 NumPy 배열(Array)로 변환할 수 있습니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
import numpy as np

# 기본 공백으로 구분된 파일 읽기
data1 = np.loadtxt('array3.txt')
# 콤마(,)로 구분된 CSV 파일 읽기
data2 = np.loadtxt('test.csv', delimiter=',')

print(data1)
print(data2)
```

## **🛠️ `np.loadtxt()` 함수의 기본 형식**
`np.loadtxt()`는 데이터를 읽어올 때 파일의 형식을 맞추기 위해 다음과 같은 인자들을 사용합니다.

```python
data = np.loadtxt('filename', delimiter='', skiprows=0, comments='#', dtype=float)
```

- `'filename'`: 불러오고자 하는 파일의 이름(혹은 경로)입니다.

- `delimiter`: 데이터가 무엇으로 구분되어 있는지 알려줍니다. 저장할 때 사용한 구분자와 반드시 일치해야 합니다. (예: CSV 파일은 ,)

- `skiprows`: 파일의 처음 몇 줄을 제외하고 읽을지 결정합니다. 제목 줄(Header)에 #이 붙어있지 않아 에러가 날 경우, skiprows=1을 사용해 첫 줄을 건너뛸 수 있습니다.

- `comments`: 주석으로 시작하는 기호를 지정합니다. 기본값은 #이며, 이 기호로 시작하는 줄은 데이터로 읽지 않고 자동으로 건너뜁니다. 프로그램에 따라 주석 기호가 다를 수 있습니다. 예를 들어 MATLAB 같은 프로그램은 주석으로 `%`를 사용하곤 합니다. 이럴 때는 comments 인자를 사용해 직접 지정해주면 됩니다.

- `dtype`: 데이터를 어떤 자료형으로 읽을지 정합니다. 기본값은 실수형(float)입니다.


---

## **데이터 가공과 컬럼 추가**

실제 프로젝트에서는 이미 존재하는 데이터 파일을 읽어와 특정 수치를 계산하고, 그 결과를 다시 파일에 합쳐서 저장하는 작업이 빈번하게 일어납니다. 

**밑변(Base)**과 **높이(Height)**가 적힌 파일을 읽어와 **넓이(Area)**를 계산하고, 이를 새로운 열(Column)로 추가하는 과정을 통해 연습해 봅시다.

## **1. 데이터 파일 만들기**

실습을 위해 먼저 20개의 사각형 정보가 담긴 `base_height.txt` 파일을 생성합니다.

```python
import numpy as np

# 1~10 사이의 정수 난수로 20행 2열 데이터 생성
initial_data = np.random.randint(1, 11, (20, 2))

# 파일로 저장 (원본 데이터)
np.savetxt('base_height.txt', initial_data, fmt='%d', header='Base Height')
```

## **2. 데이터 가공: 파일 읽기와 넓이 계산**

이제 생성된 파일을 다시 읽어와서 넓이를 계산해 봅시다.

```python
# 1. 파일 읽어오기
data = np.loadtxt('base_height.txt')

# 2. 각 열을 분리하여 변수에 담기 (슬라이싱)
base = data[:, 0]    # 모든 행의 0번째 열 (Base)
height = data[:, 1]  # 모든 행의 1번째 열 (Height)

# 3. 넓이 계산
area = base * height
```

## **3. 데이터 컬럼 추가 및 저장: 3가지 전략**
가공된 area 데이터를 기존 데이터 옆에 붙여서 3열(Base, Height, Area)로 만들어 저장해 봅시다.

### **방법 ①: 반복문 방식 (for loop)**
파일 시스템이 데이터를 한 줄씩 읽고 쓰는 원리를 이해하기에 좋습니다. 다만 속도는 느립니다.


### **🚀 TRY IT!**
{: .text-blue-200 }
```python
with open('area.txt', 'w') as f:
    f.write("# Base\tHeight\tArea\n") # 헤더 작성
    for i in range(len(base)):
        # 계산된 넓이를 포함하여 탭(\t)으로 구분해 기록
        line = f"{int(base[i])}\t{int(height[i])}\t{int(area[i])}\n"
        f.write(line)
```

### **방법 ②: vstack + Transpose 방식**
데이터를 행으로 층층이 쌓은 뒤 뒤집는 (transpose) 접근 방식입니다.

### **🚀 TRY IT!**
{: .text-blue-200 }
```python
# 가로로 3줄 쌓은 후 뒤집기 (.T)
combined = np.vstack((base, height, area)).T

# txt 파일로 저장 (fmt='%d'로 정수 지정)
np.savetxt('area2.txt', combined, fmt='%d', header='Base Height Area')
```

### **방법 ③: column_stack 방식 (권장)**

배열들을 바로 '열(Column)' 단위로 묶어주는 가장 직관적인 방법입니다. 상황에 따라 아래 두 가지 방식을 모두 편하게 사용할 수 있습니다.

**A. 1차원 배열들을 각각 조립할 때**
각각의 데이터가 따로 존재할 때 유용합니다.

```python
final_result = np.column_stack((base, height, area))
np.savetxt('area3.txt', final_result, fmt='%d', header='Base Height Area')
```

**B. 기존 2차원 데이터에 새로운 열만 추가할 때**
원본 데이터를 유지하면서 추가되는 열만 바로 이어 붙이고 싶을 때 간결하게 사용합니다.

```python
final_result = np.column_stack((data, area))
np.savetxt('area3.txt', final_result, fmt='%d', header='Base Height Area')
```

---
## **✍️ 연습 문제**

신소재공학과 학생들의 전공 과목 성적 데이터를 생성하고, 과목별/학생별 평균을 산출하여 관리하는 프로그램을 작성해 봅시다.

### **1. 성적 데이터 생성 및 저장**

우선 아래 Python 코드를 사용하여 20명의 학생에 대해 4개의 전공 과목(재료과학, 유기화학, 물리화학, 고체물리) 점수를 임의로 생성하여 `score.txt` 파일로 저장해 봅시다.

### **🚀 TRY IT!**
{: .text-blue-200 }
```python
import numpy as np

# 1. 20행 5열 배열 초기화
scores = np.zeros((20, 5))

# 2. 데이터 채우기
scores[:, 0] = np.arange(1, 21) # 학생 ID (1~20)
scores[:, 1:] = np.random.randint(0, 101, (20, 4)) # 4과목 점수 생성

# 3. 파일 저장
header = "ID MatSci Organic PhysChem SolidState"
np.savetxt('score.txt', scores, fmt='%d', header=header)

print("score.txt 파일이 생성되었습니다.")
```

위와 같이 `np.random.randint`를 사용하면 한번에 원하는 크기의 배열에 random한 숫자를 채워넣을 수 있습니다. 하지만 이중 for문 연습을 위해 직접 요소 하나하나에 접근하면서 요소들에 임의의 숫자를 대입해봅시다.

```python
import numpy as np
import random

scores = np.zeros((20, 5))

for i in range(______):
    for j in range(______):
        if ______:           # 첫번째 열의 경우 점수가 아닌 학생 index
            _______________  # random 숫자가 아닌 1부터 20까지를 순서대로 할당
        else:
            _______________  # 점수 부분은 random 숫자 할당

header = "ID MatSci Organic PhysChem SolidState"
np.savetxt('score.txt', scores, fmt='%d', header=header)

print("score.txt 파일이 생성되었습니다.")
```