---
layout: default
title: 연습 문제
parent: Ch3. 함수
nav_order: 3
---

## **✍️ 연습 문제**

### **1. 섭씨를 화씨로 변환하는 함수**
온도 데이터를 처리할 때 자주 사용하는 변환식을 함수로 만들어 봅시다. 섭씨(Celsius)를 입력받아 화씨(Fahrenheit)로 돌려주는 함수를 완성하세요.

- 변환식: F = C ✕ 1.8 + 32

```python
# 1. 함수 정의하기 (매개변수 c 설정)
def celsius_to_fahrenheit(c):
    f = 
    return 

# 2. 함수 호출 및 결과 출력
c_temp = float(input("섭씨 온도를 입력하세요: "))
result = 
print(f"섭씨 {c_temp}도는 화씨로 {result}도입니다.")
```

<details markdown="1">
<summary>예시 풀이</summary>

```python
# 1. 함수 정의하기 (매개변수 c 설정)
def celsius_to_fahrenheit(c):
    f = c * 1.8 + 32  # 변환 공식 적용
    return f          # 계산된 화씨 온도를 반환

# 2. 함수 호출 및 결과 출력
c_temp = float(input("섭씨 온도를 입력하세요: "))

# 정의한 함수를 호출하여 결과를 result 변수에 저장
result = celsius_to_fahrenheit(c_temp)

print(f"섭씨 {c_temp}도는 화씨로 {result:.1f}도입니다.")
```

</details>

### **2. 원의 넓이와 둘레 한 번에 구하기**
하나의 입력값(반지름)을 받아 두 개의 결과값(둘레, 넓이)을 반환하는 함수를 만들어 봅시다. (튜플 언패킹 활용)

- pi 값은 `math.pi`를 사용하세요.

```python
import math

def circle_info(radius):
    area =                         # 넓이 구하기
    circumference =                # 둘레 구하기
    # 1. 두 값을 한 번에 반환하기
    return 

# 2. 두 개의 변수로 나누어 받기 (Unpacking)
r = float(input("반지름을 입력하세요: "))
a, c = 

print(f"넓이: {a:.2f}, 둘레: {c:.2f}")
```

<details markdown="1">
<summary>예시 풀이</summary>

```python
import math

def circle_info(radius):
    area = math.pi * radius**2             # 넓이 구하기
    circumference = 2 * math.pi * radius   # 둘레 구하기
    
    # 1. 두 값을 한 번에 반환하기: 쉼표(,)를 사용해 여러 값을 나열하면 튜플(Tuple) 형태로 한 번에 반환됩니다.
    return area, circumference

# 사용자로부터 반지름을 입력받아 실수형(float)으로 변환
r = float(input("반지름을 입력하세요: "))

# 2. 두 개의 변수로 나누어 받기 (Unpacking): 함수가 반환한 튜플의 요소들을 각각 변수 a와 c에 순서대로 할당합니다.
a, c = circle_info(r)

print(f"넓이: {a:.2f}, 둘레: {c:.2f}")
```

</details>

### **3. 실험 데이터 통합 분석**
실험을 통해 얻은 여러 개의 측정값을 한 번에 입력받아, 데이터의 개수, 합계, 최댓값, 최솟값을 모두 반환하는 종합 분석 함수를 완성하세요.

- 파이썬의 내장 함수인 len(), sum(), max(), min()을 활용하세요.

```python
def analyze_data(*data):
    count =               # 데이터의 개수
    total =               # 전체 합계
    high  =               # 최댓값
    low   =               # 최솟값
    
    # 2. 네 개의 결과를 한 번에 반환 (튜플 형태)
    return 

# 3. 함수 호출 및 결과 언패킹 (Unpacking)


print(f"분석 데이터 개수: {n}개")
print(f"전체 합계: {s}")
print(f"최고 측정치: {ma}")
print(f"최저 측정치: {mi}")
```

<details markdown="1">
<summary>예시 풀이</summary>

```python
def analyze_data(*data):
    # 1. 내장 함수를 이용한 데이터 분석
    count = len(data)     # 데이터의 개수 (입력받은 인자의 개수)
    total = sum(data)     # 전체 합계 (데이터들의 총합)
    high  = max(data)     # 최댓값 (데이터 중 가장 큰 값)
    low   = min(data)     # 최솟값 (데이터 중 가장 작은 값)
    
    # 2. 네 개의 결과를 한 번에 반환 (튜플 형태로 반환됨)
    return count, total, high, low

# 3. 함수 호출 및 결과 언패킹 (Unpacking)
# 가변 인자(*data)이므로 여러 개의 값을 직접 쉼표로 나열하여 전달할 수 있습니다.
n, s, ma, mi = analyze_data(12.5, 14.8, 10.2, 16.5, 13.1)

print(f"분석 데이터 개수: {n}개")
print(f"전체 합계: {s}")
print(f"최고 측정치: {ma}")
print(f"최저 측정치: {mi}")
```

</details>


### **4. 계산기 프로그램의 3가지 설계 방식**
사용자로부터 숫자 2개를 입력받아 더한 결과를 출력하는 프로그램을 다음 3가지 조건과 아래 실행 결과에 맞춰 완성해 보세요. 

> {: .result .fs-3 }
> > 첫 번째 숫자: 1   
> > 두 번째 숫자: 2   
> > 결과: 3

### **4-1 입력값(X) / 반환값(X)**
함수 내부에서 모든 작업(input, print)을 처리하는 방식입니다. 호출만 하면 정해진 절차가 실행됩니다.

```python
def add_v1():
    # 1. 함수 내부에서 input()함수로 직접 입력 받기
    num1 = 
    num2 = 
    
    # 2. 결과 바로 출력하기

# 호출
add_v1()
```

<details markdown="1">
<summary>예시 풀이</summary>

```python
def add_v1():
    # 1. 함수 내부에서 input()함수로 직접 입력 받기
    # input()으로 받은 값은 문자열이므로 계산을 위해 int()로 변환해줍니다.
    num1 = int(input("첫 번째 숫자: "))
    num2 = int(input("두 번째 숫자: "))
    
    # 2. 결과 바로 출력하기
    # 함수 내부에서 계산과 출력을 한 번에 처리합니다.
    print(f"결과: {num1 + num2}")

# 함수 호출
add_v1()
```

</details>

### **4-2 입력값(O) / 반환값(O)**
데이터를 외부에서 받아 처리하고, 결과물을 다시 밖으로 던져주는 가장 전형적인 '계산기' 형태의 함수입니다.

```python
def add_v2(a, b):
    # 입력받은 두 값을 더해 결과를 반환
    return 

# 호출 및 출력
n1 = 
n2 = 
result = 
print(f"결과: {result}")
```

<details markdown="1">
<summary>예시 풀이</summary>

```python
def add_v2(a, b):
    # 입력받은 두 매개변수 a와 b를 더한 결과값을 return 키워드를 통해 함수 밖으로 던져줍니다.
    return a + b

# 호출 및 출력
# 1. 함수 외부에서 계산에 필요한 데이터를 입력 받습니다.
n1 = int(input("첫 번째 숫자: "))
n2 = int(input("두 번째 숫자: "))

# 2. 함수 add_v2를 호출하며 입력받은 값을 전달하고, 돌아온 결과값을 result 변수에 저장합니다.
result = add_v2(n1, n2)

print(f"결과: {result}")
```

</details>

### **4-3 기능의 분리 (모듈화 설계)**
'입력 받는 기능'과 '더하는 기능'을 별도의 함수로 분리하여 설계해 봅시다. 프로그램이 복잡해질수록 이처럼 기능을 쪼개는 것이 중요합니다.

```python
# 1. 입력을 담당하는 함수 (두 값을 튜플로 반환)
def get_inputs():
    a = 
    b = 
    return 

# 2. 덧셈을 담당하는 함수
def add_v3(a, b):
    return 

# 3. 두 함수를 연결하여 실행
x, y =                # 입력 함수 호출 (Unpacking)
final_result =        # 덧셈 함수 호출
print(f"결과: {final_result}")
```

<details markdown="1">
<summary>예시 풀이</summary>

```python
# 1. 입력을 담당하는 함수 (사용자로부터 두 값을 받아 튜플 형태로 반환)
def get_inputs():
    # 입력받은 값을 정수형(int)으로 변환하여 변수에 저장합니다.
    a = int(input("첫 번째 숫자: "))
    b = int(input("두 번째 숫자: "))
    # 두 값을 쉼표로 연결하여 한 번에 반환(Tuple Unpacking 활용)합니다.
    return a, b

# 2. 덧셈만을 담당하는 계산용 함수
def add_v3(a, b):
    # 전달받은 두 매개변수를 더해 결과만 깔끔하게 돌려줍니다.
    return a + b

# 3. 두 함수를 연결하여 실행 (입력 따로, 계산 따로 처리)
x, y = get_inputs()          # 입력 함수를 호출하여 결과값을 x와 y에 나누어 담습니다.
final_result = add_v3(x, y)  # 추출된 x, y를 덧셈 함수에 전달하여 최종 결과를 얻습니다.

print(f"결과: {final_result}")
```

</details>

### **💡3가지 방식의 차이**
- **4-1**은 재사용성이 떨어지고 항상 입력을 새로 받아야 합니다.
- **4-2**는 이미 있는 데이터를 처리하기 좋습니다.
- **4-3**은 실제 대규모 프로젝트에서 쓰이는 구조로, 입력 방식이 바뀌라도 더하는 함수는 수정할 필요가 없다는 모듈화의 장점이 있습니다.