---
layout: default
title: 파일 입출력 실습 (1)
parent: Ch5. 파일 입출력
nav_order: 3
---

# **파일 입출력 실습 (1)**

## 📂 실습 데이터 다운로드
실습을 시작하기 전, 아래 파일들을 다운로드하여 사용하세요.

- [score.txt 다운로드](./data/score.txt){:download}
- [ID.txt 다운로드](./data/ID.txt){:download}
- [triangle.txt 다운로드](./data/triangle.txt){:download}

> {: .highlight }
링크를 클릭했을 때 파일 내용이 화면에 보인다면, 마우스 오른쪽 버튼을 클릭한 뒤 **다른 이름으로 링크 저장**을 선택하세요!

---

## **1. `readline()`과 `while True`를 사용해 한 줄씩 읽기**

파일의 끝(EOF)을 직접 확인하며 데이터를 한 줄씩 불러오는 고전적인 파일 읽기 방식을 이해합니다.

- 학생과 점수 두 열로 구성된 `score.txt` 파일을 읽기 모드로 엽니다.
- `while True` 루프와 `readline()`을 사용하여 파일 끝까지 한 줄씩 읽습니다.
- `if not line: break`를 사용하여 파일의 끝에서 루프에서 탈출합니다.
- 점수가 80점 이상인 학생에 대해서만 통과했다는 메시지를 출력합니다.

```python
f = open('./score.txt', 'r')

while True:
    line = f.readline()
    if not line: break # 더 읽을 줄이 없으면 반복문 탈출
    
    tmp = line.split()
    name = tmp[0]
    score = float(tmp[1])
    
    if score >= 80:
        print(f'{name} passes the course')

f.close()
```

## **2. `readlines()`와 `while` 문을 사용해 읽기 (리스트 활용)**

파일 전체를 메모리(리스트)에 한꺼번에 불러와 인덱스로 제어하는 방식을 익힙니다.

- 1번과 동일하게 `score.txt` 파일을 읽어 점수가 80점 이상인 학생에 대해서만 통과했다는 메시지를 출력합니다.
- `readlines()`를 사용하여 파일의 모든 행을 리스트 형태로 가져옵니다.
- `while` 문과 인덱스 변수 `i`를 사용하여 리스트의 요소에 하나씩 접근합니다.
- 루프가 리스트의 길이(`len`)를 넘지 않도록 제어하며 데이터를 처리합니다.

```python
f = open('./score.txt', 'r')
lines = f.readlines()  # 파일 전체를 리스트로 가져옴
i = 0

while i < len(lines):
    line = lines[i]
    tmp = line.split()
    name = tmp[0]
    score = float(tmp[1])
    
    if score >= 80:
        print(f'{name} passes the course')
    
    i += 1  # 다음 줄로 넘어가기 위한 인덱스 증가

f.close()
```


---

## **3. `for line in f`를 이용한 파일 읽기**

파이썬에서 가장 권장되는 표준적인 파일 읽기 방식입니다. `readline()`을 명시적으로 호출하지 않아도 `for` 문이 알아서 줄 단위로 데이터를 공급해 줍니다.

- 이름과 주민등록번호 두 열로 구성된 `ID.txt` 파일을 읽기 모드로 엽니다.
- 별도의 `readline()` 호출 없이 for 문에 파일 객체를 직접 넣어 한 줄씩 꺼냅니다.
- 주민등록번호의 **8번째 글자(인덱스 7)**를 슬라이싱하여 성별을 판별합니다.
- '1' 또는 '3'은 남성, 그 외는 여성으로 판별하여 결과를 출력합니다.

```python
f = open('ID.txt', 'r')

# 파일 객체 자체를 루프에 넣으면 한 줄씩 꺼내줍니다.
for line in f:
    tmp = line.split()
    name, ID = tmp[0], tmp[1]
    
    # 주민번호 뒷자리 첫 번째 숫자(인덱스 7)로 판별
    if ID[7] in ['1', '3']:
        print(f'{name} is male')
    else:
        print(f'{name} is female')

f.close()
```

---
## **4. `with open`을 이용한 파일 읽기 및 저장**

`close()`를 명시하지 않아도 안전하게 파일을 관리하는 `with` 문의 사용법과 여러 파일을 동시에 제어하는 법을 배웁니다.

- 3번과 동일하게 이름과 주민등록번호 두 열로 구성된 `ID.txt` 파일을 처리합니다.
- 이 때 `with` 문 하나에 쉼표(`,`)를 사용하여 입력 파일(r)과 출력 파일(w) 2개를 동시에 엽니다.
- 남성 데이터는 `male.txt`에, 여성 데이터는 `female.txt`에 각각 나누어 저장합니다.

```python
# 읽기용 f, 쓰기용 m(남성), w(여성) 파일을 동시에 엽니다.
with open('ID.txt', 'r') as f, \
     open('male.txt', 'w') as m, \
     open('female.txt', 'w') as w:

    for line in f:
        tmp = line.split()
        if tmp[1][7] in ['1', '3']:
            m.write(line)
        else:
            w.write(line)
# 블록이 끝나면 close()를 하지 않아도 파일이 자동으로 닫힙니다.
```

---
## **5. 헤더 건너뛰기와 데이터 가공**

실제 공학 데이터 파일은 첫 줄에 데이터의 제목(Header)이 있는 경우가 많습니다. 이를 처리하고 수치 데이터를 처리해 새로운 파일에 저장해봅시다.

- 삼각형의 밑변과 높이 두 열로 구성된 `triangle.txt` 파일을 처리합니다.
- 데이터 위에 있는 불필요한 줄들을 `readline()`으로 건너뜁니다.
- 삼각형의 넓이를 계산한 뒤, 결과 파일에는 밑변, 높이, 넓이를 적습니다.
- 새 파일에는 'Base', 'Height', 'Area' 헤더를 새로 작성합니다.

```python
with open('triangle.txt', 'r') as f, open('area.txt', 'w') as out:
    # 1. 첫 번째 줄(Base Height) 읽어서 건너뛰기
    header = f.readline()
    
    # 2. 결과 파일에도 제목 작성
    out.write('Base\tHeight\tArea\n')
    
    # 3. 두 번째 줄부터는 수치 데이터이므로 루프 처리
    for line in f:
        tmp = line.split()
        b = float(tmp[0])
        h = float(tmp[1])
        area = 0.5 * b * h
        
        # 탭(\t)으로 구분하여 소수점 둘째 자리까지 저장
        out.write(f'{b:.2f}\t{h:.2f}\t{area:.2f}\n')

print("area.txt 파일에 계산 결과가 저장되었습니다.")
```