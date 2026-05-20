---
layout: default
title: 파일 입출력 실습 (3)
parent: Ch5. 파일 입출력
nav_order: 5
---

# **파일 입출력 실습 (3)**

---

지난 시간과 다르게 이번에는 **학과마다 학생 수가 다른** 데이터를 다룹니다. 구조는 `toeic.txt`와 같지만, 학과 주석줄에 적힌 인원수가 제각각입니다.
학생 수가 정해져 있지 않으므로 for 문에 고정된 횟수를 넣을 수 없습니다. 이번 실습에서는 while문으로 빈줄 또는 다음 구분자가 나올 때까지 읽는 방식을 배웁니다. 데이터를 NumPy 배열에 담아야 할 경우에는 NaN으로 초기화한 배열을 먼저 만들고 채워넣는 방식을 사용합니다.

---

## 📂 실습 데이터 다운로드

- [toeic2.txt 다운로드](https://bgjang-khu.github.io/MSE201/Ch5/data/toeic2.txt)

```
# 경희대학교 공과대학
# 9개 학과, 총 546명의 토익 점수 데이터 (학과별 인원 상이)

# 기계공학과 90명  시험 일시: 2025.03.15
840
625
...  (90줄)

# 산업경영공학과 62명  시험 일시: 2025.03.22
825
840
...  (62줄)

...
```

---

## 1. `find_max_std(filename)` — 최다 인원 학과 파악하기

### 목표

`toeic2.txt`를 읽으면서 각 학과의 학생 수를 출력하고, **가장 학생 수가 많은 학과와 인원수**를 찾아 반환하는 함수를 작성합니다. 이 값은 다음 함수에서 배열 크기를 결정하는 데 사용합니다.

- 학과 주석줄(`# 기계공학과 90명 ...`)에서 `split()`으로 학과명과 인원수를 직접 읽어옵니다.
- 인원수를 비교하며 최댓값과 해당 학과명을 갱신합니다.
- 학과명 리스트(`dept_names`)와 최대 인원수(`max_n`)를 함께 반환합니다.

### 출력 예시

> {: .result .fs-3 }
기계공학과: 90명  
산업경영공학과: 62명  
원자력공학과: 46명  
화학공학과: 70명  
신소재공학과: 65명  
사회기반시스템공학과: 63명  
건축공학과: 67명  
환경학및환경공학과: 39명  
건축학과: 44명  
최다 인원 학과: 기계공학과 (90명)

<details markdown="1">
<summary>예시 풀이</summary>


```python
import numpy as np

def find_max_std(filename):
    f = open(filename, 'r', encoding='utf-8')
    f.readline(); f.readline(); f.readline()

    max_n = 0
    max_dept = ''
    dept_names = []

    for i in range(9):
        line = f.readline()             # '# 기계공학과 90명  시험 일시: ...'
        dept = line.split()[1]
        n = int(line.split()[2][:-1])   # '90명' → [:-1]로 '명' 제거 → int 변환
        dept_names.append(dept)
        print(f'{dept}: {n}명')

        if n > max_n:
            max_n = n
            max_dept = dept

        for j in range(n):              # n줄만큼 점수 건너뛰기
            f.readline()
        f.readline()                    # 빈줄 건너뛰기

    f.close()
    print(f'최다 인원 학과: {max_dept} ({max_n}명)')
    return max_n, dept_names


#### 실행 부분 ####

max_n, dept_names = find_max_std('toeic2.txt')
```
</details>

---

## 1-2. `while`문으로 인원수 직접 세기

주석줄에 인원수 정보가 없어도 동작하는 방식입니다. `while`문으로 다음 빈줄이 나올 때까지 줄을 읽으며 직접 카운트합니다.

<details markdown="1">
<summary>예시 풀이</summary>

```python
def find_max_std_v2(filename):
    f = open(filename, 'r', encoding='utf-8')
    f.readline(); f.readline(); f.readline()

    max_n = 0
    max_dept = ''
    dept_names = []

    for i in range(9):
        dept = f.readline().split()[1]
        dept_names.append(dept)

        count = 0
        while True:
            line = f.readline()
            if line.strip() == '' or line.strip().startswith('#'):
                break           # 빈줄 또는 다음 학과 주석줄이 나오면 탈출
            count += 1

        print(f'{dept}: {count}명')

        if count > max_n:
            max_n = count
            max_dept = dept

    f.close()
    print(f'최다 인원 학과: {max_dept} ({max_n}명)')
    return max_n, dept_names


#### 실행 부분 ####

max_n, dept_names = find_max_std_v2('toeic2.txt')
```
</details>

---

## 2. `load_toeic2(filename, max_n)` — NaN 배열로 불러오기

### 목표

1번에서 구한 `max_n`을 받아 `(max_n, 9)` 크기의 NaN 배열을 만들고, 각 학과의 점수를 채워넣은 뒤 인덱스 열을 붙여 반환하는 함수를 작성합니다.

- `np.full((max_n, 9), np.nan)`으로 배열을 초기화합니다.
- 학과 주석줄에서 인원수 `n`을 읽어 `range(n)`만큼만 점수를 채웁니다. 나머지는 NaN 그대로 유지됩니다.
- 인덱스 열(1~`max_n`)을 `np.column_stack()`으로 맨 앞에 붙입니다.

### 배열 구조

```
index  기계공학과  산업경영공학과  ...  건축학과
1      840        825            ...  780
2      625        840            ...  700
...
62     ...        795            ...  NaN   ← 산업경영공학과 마지막
63     ...        NaN            ...  NaN   ← 이후는 NaN
...
90     735        NaN            ...  NaN   ← 기계공학과만 존재
```


{: .highlight }
> 💡 **TIP — `np.full()`**
>
> `np.full(shape, value)`를 사용하면 원하는 값으로 배열을 초기화할 수 있습니다. 여기서는 결측값을 나타내는 `np.nan`으로 초기화하여, 데이터가 없는 자리를 명시적으로 표현합니다.
>
> ```python
> np.zeros((3, 3))          # 0으로 채우기
> np.ones((3, 3))           # 1로 채우기
> np.full((3, 3), np.nan)   # NaN으로 채우기
> np.full((3, 3), -1)       # 원하는 값으로 채우기
> ```

<details markdown="1">
<summary>예시 풀이</summary>

```python
def load_toeic2(filename, max_n):
    f = open(filename, 'r', encoding='utf-8')
    f.readline(); f.readline(); f.readline()

    data = np.full((max_n, 9), np.nan)   # NaN으로 초기화

    for col in range(9):
        line = f.readline()
        n = int(line.split()[2][:-1])    # 학과 인원수 읽기
        for row in range(n):             # n명만큼만 채우기
            data[row][col] = int(f.readline().strip())
        f.readline()                     # 빈줄 건너뛰기

    # 인덱스 열(1~max_n) 붙이기
    idx = np.arange(1, max_n + 1)
    data = np.column_stack((idx, data))

    f.close()
    print(f'배열 생성 완료: {data.shape}')   # (90, 10)
    return data


#### 실행 부분 ####

data = load_toeic2('toeic2.txt', max_n)
```
</details>

---

## 3. `print_stats(data, dept_names)` — 학과별 평균·표준편차 출력

### 목표

2번에서 만든 배열을 받아 각 학과의 평균과 표준편차를 출력하는 함수를 작성합니다.

- `data`의 0번째 열은 인덱스이므로, `j=1`부터 순회합니다.
- NaN이 섞여 있으므로 `np.nanmean()`과 `np.nanstd()`를 사용합니다.

### 출력 예시

> {: .result .fs-3 }
> 학과          평균        표준편차  
> \----------------------------------------  
> 기계공학과            668.2       103.5  
> 산업경영공학과            757.3       87.0  
> 원자력공학과          715.8       104.8  
> 화학공학과        721.3       81.1  
> 신소재공학과          793.8       97.3  
> 사회기반시스템공학과          627.8       110.1  
> 건축공학과            676.4       101.6  
> 환경학및환경공학과            748.5       84.7  
> 건축학과          704.8       97.7  


{: .highlight }
> 💡 **TIP — `np.nanmean()` / `np.nanstd()`**
>
> NaN이 섞인 배열에 일반 `mean()`이나 `std()`를 쓰면 결과도 NaN이 나옵니다. `np.nanmean()`과 `np.nanstd()`는 **NaN을 무시하고** 실제 데이터만으로 계산합니다.
>
> ```python
> col = np.array([800., 750., np.nan, np.nan])
> print(col.mean())        # nan
> print(np.nanmean(col))   # 775.0
> ```

<details markdown="1">
<summary>예시 풀이</summary>

```python
def print_stats(data, dept_names):
    n_row, n_col = data.shape

    print(f'{"학과":<20}  {"평균":>8}  {"표준편차":>8}')
    print('-' * 45)

    for j in range(1, n_col):       # 0번째 열은 인덱스이므로 1부터 시작
        col = data[:, j]
        avg = np.nanmean(col)
        std = np.nanstd(col)
        print(f'{dept_names[j-1]:<20}  {avg:>8.1f}  {std:>8.1f}')


#### 실행 부분 ####

print_stats(data, dept_names)
```
</details>

---

## 🚀 전체 실행

```python
max_n, dept_names = find_max_std('toeic2.txt')
data = load_toeic2('toeic2.txt', max_n)
print_stats(data, dept_names)
```
---
## **✍️ 연습 문제**

## **연습 문제 1 — 기온 데이터 처리 (`temp2024.txt`)**

경희대학교 국제캠퍼스 인근에서 2024년 한 해 동안 측정한 기온 데이터입니다. 월마다 측정 횟수가 다르므로, `for`문에 고정된 횟수를 넣을 수 없습니다. `while`문으로 빈줄이 나올 때까지 읽는 방식으로 처리합니다.

- [temp2024.txt 다운로드](https://bgjang-khu.github.io/MSE201/Ch5/data/temp2024.txt)


각 데이터 줄은 `날짜:온도:날씨` 형식입니다.

---

### **1-1 `print_monthly_avg(filename)` — 월별 평균 기온 출력 함수 작성**

`temp2024.txt`를 읽으면서 월별 평균 기온과 측정 횟수를 출력하는 함수를 작성합니다.

- 함수: `print_monthly_avg(filename)` - 월별 평균 기온 출력
    - 입력: `filename` - 읽을 파일 이름
    - 반환: 없음
- 헤더 3줄(주석 2줄 + 빈줄 1줄)을 건너뜁니다.
- 12개월을 `for`문으로 순회하고, 각 월 안에서는 `while`문으로 빈줄이 나올 때까지 읽습니다.
- `split(':')`으로 온도를 파싱하고, `[:-1]`로 `C`를 제거한 뒤 `float`으로 변환합니다.
- 측정값들을 리스트에 담아 평균을 계산하고 출력합니다.

**출력 예시**

```
1월 평균 기온: -2.0C (23회 측정)
2월 평균 기온: 1.9C (23회 측정)
...
11월 평균 기온: 6.2C (19회 측정)
12월 평균 기온: -0.4C (20회 측정)
```

```python
def print_monthly_avg(filename):
    f = open(filename, 'r', encoding='utf-8')

    함수를 완성하세요.

#### 실행 부분 ####

print_monthly_avg('temp2024.txt')
```


<!-- 
**답안**

```python
def print_monthly_avg(filename):
    f = open(filename, 'r', encoding='utf-8')
    f.readline(); f.readline(); f.readline()   # 헤더 3줄 건너뛰기

    for month in range(1, 13):
        f.readline()                           # # N월 건너뛰기
        temps = []

        while True:
            line = f.readline()
            if line.strip() == '' or line.strip().startswith('#'):
                break                          # 빈줄 또는 다음 월 주석이면 탈출
            temp = float(line.strip().split(':')[1][:-1])   # '2.3C' → 2.3
            temps.append(temp)

        avg = sum(temps) / len(temps)
        print(f'{month}월 평균 기온: {avg:.1f}C ({len(temps)}회 측정)')

    f.close()


#### 실행 부분 ####

print_monthly_avg('temp2024.txt')
```
-->

---

### **1-2 `save_hot_days(filename, outfilename)` — 30도 초과 날짜 저장 함수 작성**

`temp2024.txt`를 읽으면서 기온이 **30도를 초과하는 날짜**만 골라 새로운 파일에 저장하는 함수를 작성합니다.

- - 함수: `save_hot_days(filename, outfilename)` - 30도 초과 날짜 저장
    - 입력: `filename` — 읽을 파일 이름, `outfilename` — 저장할 파일 이름 (기본값: 'hot_days.txt')
    - 반환: 없음
- (1)과 같은 방식으로 파일을 읽습니다.
- 기온이 30도 초과인 줄만 필터링하여 저장합니다.
- 저장 형식은 원본과 다르게 **공백으로 구분**하여 저장합니다.
- 저장된 날짜 수를 count로 세어 마지막에 **출력**합니다.

**저장 파일 형식 (`hot_days.txt`)**

```
# 30도 초과 측정일 목록
# date temp weather
2024-07-05 30.4C Rainy
2024-07-12 30.9C Overcast
2024-07-24 31.0C Cloudy
2024-08-19 30.1C Sunny
```

```python
def save_hot_days(filename, outfilename='hot_days.txt'):
    f = open(filename, 'r', encoding='utf-8')

    함수를 완성하세요.

#### 실행 부분 ####

save_hot_days('temp2024.txt')
```

<!-- 

```python
def save_hot_days(filename, outfilename='hot_days.txt'):
    f = open(filename, 'r', encoding='utf-8')
    out = open(outfilename, 'w', encoding='utf-8')

    f.readline(); f.readline(); f.readline()   # 헤더 3줄 건너뛰기

    out.write('# 30도 초과 측정일 목록\n')
    out.write('# date temp weather\n')

    count = 0
    for month in range(1, 13):
        f.readline()                           # # N월 건너뛰기

        while True:
            line = f.readline()
            if line.strip() == '' or line.strip().startswith('#'):
                break
            parts = line.strip().split(':')
            date    = parts[0]
            temp    = float(parts[1][:-1])
            weather = parts[2]

            if temp > 30:
                out.write(f'{date} {temp}C {weather}\n')
                count += 1

    f.close()
    out.close()
    print(f'저장 완료: {outfilename} ({count}일)')


#### 실행 부분 ####

save_hot_days('temp2024.txt')
```
 --> 