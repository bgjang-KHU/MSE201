---
layout: default
title: 파일 입출력 실습 (2)
parent: Ch5. 파일 입출력
nav_order: 4
---

# **파일 입출력 실습 (2)**

---

실습 (1)에서 한 줄씩 읽어 데이터를 처리하는 방법을 배웠다면, 이번 실습에서는 **규칙적이지만 복잡한 형식**의 파일을 파싱하고, 읽어온 데이터를 **NumPy 배열**로 변환한 뒤 다양한 형태로 가공·저장하는 방법을 배웁니다.

각 소문항은 **하나의 함수**로 구현합니다. 함수를 잘 설계하면 코드를 재사용하기 쉽고, 나중에 수정할 때도 편리합니다.

---

## 📂 실습 데이터 다운로드

실습을 시작하기 전, 아래 파일을 다운로드하여 사용하세요.

- [toeic.txt 다운로드](https://bgjang-khu.github.io/MSE201/Ch5/data/toeic.txt)

링크를 클릭했을 때 파일 내용이 화면에 보인다면, 마우스 오른쪽 버튼을 클릭한 뒤 **다른 이름으로 링크 저장**을 선택하세요!

---

## 📄 데이터 파일 구조 파악하기

문제를 풀기 전, 반드시 `toeic.txt` 파일을 직접 열어 구조를 확인하세요. 파일 읽기에서 가장 중요한 첫 번째 단계는 **파일이 어떻게 생겼는지 눈으로 파악하는 것**입니다.

```
# 경희대학교 공과대학
# 9개 학과, 학과 당 50명, 총 450명의 토익 점수 데이터

# 기계공학과 50명  시험 일시: 2025.03.15
580
515
595
...  (50줄)

# 산업경영공학과 50명  시험 일시: 2025.03.22
650
580
...  (50줄)

...
```

파일 전체를 보면 다음과 같은 규칙이 보입니다.

- 맨 위: 주석 2줄 + 빈줄 1줄
- 이후 9개 학과가 반복: **학과 주석 1줄 → 점수 50줄 → 빈줄 1줄**

이 규칙을 머릿속에 그리고 나면, `readline()`으로 어떤 줄을 건너뛰어야 할지 자연스럽게 보입니다.

---

## 1. `count_pass(filename)` — 500점 이상 학생 수 세기

### 목표

`toeic.txt`를 읽으면서 각 학과별로 **500점 이상**인 학생 수를 세어 출력하는 함수를 작성합니다.

- 파일을 열고, 맨 위 헤더 3줄(주석 2줄 + 빈줄 1줄)을 `readline()`으로 건너뜁니다.
- 9개 학과를 순서대로 처리합니다. 학과마다 주석 1줄을 건너뛰고, 50개 점수를 읽으며 조건을 확인합니다.
- 각 학과가 끝나면 빈줄 1줄을 건너뜁니다.
- 학과별 결과와 전체 합산을 출력합니다. `return`은 없습니다.

### 출력 예시

```
기계공학과 500점 이상 학생: 28명
산업경영공학과 500점 이상 학생: 47명
원자력공학과 500점 이상 학생: 35명
화학공학과 500점 이상 학생: 35명
신소재공학과 500점 이상 학생: 47명
사회기반시스템공학과 500점 이상 학생: 27명
건축공학과 500점 이상 학생: 34명
환경학및환경공학과 500점 이상 학생: 47명
건축학과 500점 이상 학생: 36명
경희대학교 공과대학 500점 이상 학생 총 336명
```

### 답안

```python
import numpy as np

def count_pass(filename):
    f = open(filename, 'r', encoding='utf-8')

    # 맨 위 헤더: 주석 2줄 + 빈줄 1줄 건너뛰기
    f.readline()
    f.readline()
    f.readline()

    total = 0

    for dept_idx in range(9):
        dept = f.readline().split()[1]   # '# 기계공학과 50명 ...' → '기계공학과'
        count = 0
        for std_idx in range(50):
            score = int(f.readline().strip())
            if score >= 500:
                count += 1
        f.readline()   # 학과 뒤 빈줄 건너뛰기

        print(f'{dept} 500점 이상 학생: {count}명')
        total += count

    f.close()
    print(f'경희대학교 공과대학 500점 이상 학생 총 {total}명')


#### 실행 부분 ####

count_pass('toeic.txt')
```

---

## 2. `load_toeic(filename)` — NumPy 배열로 불러오기

### 목표

`toeic.txt`를 읽어 점수 전체를 **50행 × 9열** NumPy 배열로 저장하고 반환하는 함수를 작성합니다.

- 행(row)은 학생 번호(1~50), 열(col)은 학과를 나타냅니다.
- `np.zeros((50, 9), dtype=int)`로 빈 배열을 먼저 만들고, 이중 `for` 문으로 채웁니다.
  - 바깥 `for`문은 열(학과) 방향으로, 안쪽 `for`문은 행(학생) 방향으로 순회합니다.
- 함수는 완성된 배열을 `return`합니다.

### 🤔 잠깐, 왜 열 방향으로 먼저 순회할까요?

우리가 원하는 최종 배열은 **행 = 학생, 열 = 학과** 형태입니다. 그런데 파일은 **학과 단위로** 50개 점수가 이어서 적혀 있습니다. 즉, 파일을 읽는 순서는 열(학과) 방향이 자연스럽습니다. 이처럼 "데이터를 저장하는 순서"와 "파일에 적힌 순서"가 다를 때, 행과 열을 어떻게 인덱싱할지 잘 생각해야 합니다.

### 답안

```python
def load_toeic(filename):
    f = open(filename, 'r', encoding='utf-8')

    # 맨 위 헤더 3줄 건너뛰기
    f.readline()
    f.readline()
    f.readline()

    data = np.zeros((50, 9), dtype=int)   # 50행 9열 빈 배열

    for col in range(9):           # 학과(열) 방향으로 9번 반복
        f.readline()               # 학과 주석줄 건너뛰기
        for row in range(50):      # 학생(행) 방향으로 50번 반복
            data[row][col] = int(f.readline().strip())
        f.readline()               # 빈줄 건너뛰기

    f.close()
    return data

data = load_toeic('toeic.txt')
print(data.shape)   # (50, 9)
print(data[:3])     # 처음 3명의 점수 확인
```

### 실행 결과

```
(50, 9)
[[580 650 430 575 685 385 455 630 420]
 [515 580 525 585 705 600 485 745 520]
 [595 550 535 480 760 720 615 685 580]]
```

---

## 3. `save_by_std(data, filename)` — 학과별 점수표 저장

### 목표

2번에서 만든 배열을 받아 `np.savetxt()`로 깔끔하게 저장하는 함수를 작성합니다.

- 헤더는 9개 학과명을 탭(`\t`)으로 구분한 문자열로 작성합니다.
- `np.savetxt()`의 `delimiter`, `fmt`, `header` 인자를 활용합니다.

### 저장 파일 형식 (`toeic_by_std.txt`)

```
# 기계공학과	산업경영공학과	원자력공학과	화학공학과	신소재공학과	사회기반시스템공학과	건축공학과	환경학및환경공학과	건축학과
580	650	430	575	685	385	455	630	420
515	580	525	585	705	600	485	745	520
...
```

### 답안

```python
def save_by_std(data, filename='toeic_by_std.txt'):
    header = '\t'.join(DEPT_NAMES)   # 학과명을 탭으로 이어 붙여 헤더 생성
    np.savetxt(filename, data, fmt='%d', delimiter='\t', header=header)
    print(f'저장 완료: {filename}')

save_by_std(data)
```

---

## 4. `save_stats(data, filename)` — 인덱스 열 + 평균·표준편차 행 추가하여 저장

### 목표

2번 배열을 받아 다음 두 가지를 추가하고 저장하는 함수를 작성합니다.

- **맨 앞 열**: 학생 인덱스(1~50)
- **맨 아래 두 행**: 각 학과의 평균(`avg`)과 표준편차(`std`)

### 저장 파일 형식 (`toeic_stats.txt`)

```
# index	기계공학과	산업경영공학과	...	건축학과
1	580	650	...	420
2	515	580	...	520
...
50	355	595	...	475
avg	507.1	621.2	...	574.5
std	92.1	86.2	...	105.5
```

### 구현 전략

이 함수는 크게 세 단계로 나뉩니다.

1. **평균·표준편차 계산**: 이중 `for`문으로 열(학과) 방향으로 순회하며 계산합니다.
2. **인덱스 열 추가**: `np.arange()`와 `np.column_stack()`으로 학생 번호 열을 붙입니다.
3. **파일 저장**: 숫자 행은 `for`문으로, 마지막 두 행(`avg`, `std`)은 따로 `write()`로 작성합니다.

### 답안

```python
def save_stats(data, filename='toeic_stats.txt'):
    n = data.shape[0]   # 학생 수 = 50

    # 1. 학과별 평균과 표준편차 계산 (이중 for문)
    avg_row = np.zeros(9)
    std_row = np.zeros(9)

    for j in range(9):           # 열(학과) 방향
        total = 0
        for i in range(n):       # 행(학생) 방향
            total += data[i][j]
        avg_row[j] = total / n

        sq_sum = 0
        for i in range(n):
            sq_sum += (data[i][j] - avg_row[j]) ** 2
        std_row[j] = (sq_sum / n) ** 0.5

    # 2. 학생 인덱스 열(1~50) 붙이기
    idx = np.arange(1, n + 1).reshape(-1, 1)   # (50, 1) 형태로 변환
    data_with_idx = np.column_stack((idx, data))  # (50, 10)

    # 3. 파일 저장
    dept_header = '\t'.join(DEPT_NAMES)
    with open(filename, 'w', encoding='utf-8') as out:
        # 헤더
        out.write(f'# index\t{dept_header}\n')

        # 학생 데이터 50행
        for i in range(n):
            row = data_with_idx[i]
            out.write('\t'.join(str(int(x)) for x in row) + '\n')

        # 평균 행
        out.write('avg\t' + '\t'.join(f'{x:.1f}' for x in avg_row) + '\n')
        # 표준편차 행
        out.write('std\t' + '\t'.join(f'{x:.1f}' for x in std_row) + '\n')

    print(f'저장 완료: {filename}')

save_stats(data)
```

---

## 🚀 전체 실행

네 함수를 모두 완성했다면, 아래 코드로 한 번에 실행해 보세요.

```python
count_pass('toeic.txt')

data = load_toeic('toeic.txt')

save_by_std(data)
save_stats(data)
```
