---
layout: default
title: 함수의 응용
parent: Ch3. 함수
nav_order: 2
---

# 함수의 응용

---

## **1. 여러 개의 결과값 반환하기**

{: .warning }
**함수의 반환값은 언제나 하나입니다!**

Python에서 함수는 기술적으로 단 하나의 객체만을 반환할 수 있습니다. 하지만 return 문 뒤에 여러 값을 쉼표(,)로 구분하여 나열하면, Python은 이를 자동으로 하나의 **튜플(Tuple)**로 묶어 처리합니다. 따라서 우리 눈에는 여러 개가 반환되는 것처럼 보이지만, 실제로는 '튜플'이라는 하나의 보따리가 전달되는 것입니다.

```python 
def add_and_mul(a, b):
    return a + b, a * b  # (a+b, a*b) 형태의 튜플 하나를 반환

# 변수 하나로 받으면 튜플로 저장됨
result = add_and_mul(3, 4)
print(result)  # (7, 12)
```

### 튜플 언패킹 (Tuple Unpacking)의 활용
반환된 결과값들을 각각 독립된 변수에 담아 사용하고 싶을 때는 언패킹(Unpacking) 기법을 활용합니다. 함수의 결과값 개수와 동일하게 변수를 쉼표로 나열하여 호출하면 됩니다.

```python
# 반환된 튜플을 분리하여 각각 저장
res1, res2 = add_and_mul(3, 4)

print(res1)  # 7
print(res2)  # 12
```

### 💡 Note: 다양한 자료형의 혼합 반환
반환값에는 정수나 실수뿐만 아니라 리스트와 같은 복합 자료형도 포함될 수 있습니다. 이 경우에도 전체 결과는 하나의 튜플로 묶이며, 언패킹 시 각 변수는 해당 자료형을 그대로 유지합니다.

```python
def my_trig_sum(a, b):
    out1 = a + b
    out2 = a - b
    return out1, out2, [out1, out2] # 숫자 2개와 리스트 1개를 반환

c, d, e = my_trig_sum(2, 3) #
# e 에는 [5, -1] 이라는 리스트가 담기게 됩니다.
```

## **2. 매개변수 초깃값 설정과 지정 호출**

### 매개변수 초깃값 (Default Value)
함수를 정의할 때 매개변수에 미리 값을 대입해 두면, 호출 시 해당 인자를 생략해도 설정된 초깃값이 자동으로 적용됩니다. 특정 값이 반복적으로 사용되는 경우 코드를 간결하게 만들어 줍니다.

### 매개변수 이름을 지정하여 호출하기
함수를 호출할 때 매개변수의 이름을 직접 지정하여 값을 전달할 수 있습니다. 이 방식을 사용하면 정의된 순서와 상관없이 원하는 매개변수에 값을 정확히 전달할 수 있어 가독성이 향상됩니다.

```python
# day와 name에 초깃값 설정 
def print_greeting(name='Student', day='Monday'):
    print(f"Greetings, {name}, today is {day}")

# 1. 인자 없이 호출 (모든 초깃값 사용) 
print_greeting() # Greetings, Student, today is Monday

# 2. 특정 매개변수만 지정하여 호출 
print_greeting(name='Alex') # Greetings, Alex, today is Monday

# 3. 매개변수 이름을 지정하여 순서 변경 
print_greeting(day='Friday', name='Timmy') # Greetings, Timmy, today is Friday
```

{: .warning }
**주의: 매개변수의 배치 순서**
초깃값이 설정된 매개변수는 반드시 초깃값이 없는 매개변수보다 뒤쪽에 배치해야 합니다. 만약 def func(name='User', age):와 같이 작성하면, 호출 시 입력된 값이 어느 변수에 대입될지 판단할 수 없어 SyntaxError가 발생합니다.