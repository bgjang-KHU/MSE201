---
layout: default
title: 숫자형
parent: Ch2. 변수와 기본 자료구조
nav_order: 2
---
# 숫자형 

---

|  **항목**   |   **예시**            |
|:-------|:------------------|
|Integer (int, 정수)              | 123, -345, 0        |
|Floating-point (float, 실수)     | 123.45, 1.0, 3.4e10 |
|Complex number (complex, 복소수) | 1+2j, -3J            |

---

### **정수형**

정수형(integer)은 말 그대로 정수를 뜻하는 자료형입니다.

```python
a=123
print(a)
print(type(a))
```
> {: .result .fs-3 }
> > 123  
> > <class 'int'>

{: .tip }
내장 함수 `type()`을 사용하면 자료의 유형을 확인할 수 있습니다.


---

### **실수형**

실수형(floating-point)은 소수점이 포함된 숫자를 말합니다.

```python
b=3.4e10
print(b)
print(type(b))
```
> {: .result .fs-3 }
> > 34000000000.0  
> > <class 'float'>

---

### **복소수형**

복소수형(complex)은 실수부와 허수부로 구성된 숫자를 말하며, 파이썬에서는 허수 단위로 **j**를 사용합니다.

```python
c=1+2j
print(c)
print(type(c))
```
> {: .result .fs-3 }
> > (1+2j)  
> > <class 'complex'>

---

### **데이터 유형 바꾸기**


`1`은 정수형(int)이고, `1.0`은 실수형(float)입니다.
데이터의 유형을 바꾸는 것도 가능합니다.

```python
a=1
print(a)
print(type(a))

b=float(a)
print(b)
print(type(b))
```


> {: .result .fs-3 }
> > 1   
> > <class 'int'>  
> > 1.0  
> > <class 'float'>

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
a=1.0
print(a)
print(type(a))

b=int(a)
print(b)
print(type(b))
```

---

### **🤔 Wait and Think!**
❓ 잠깐! 그렇다면 복소수를 정수형이나 실수형으로 바꾸는 것도 가능할까요?

```python
a=1+2j

b=int(a)
print(b)
print(type(b))
```

---

### **복소수형 응용**

- complex.**real** : 복소수의 실수부를 반환합니다. (속성/Attribute)
- complex.**imag** : 복소수의 허수부를 반환합니다. (속성/Attribute)
- complex.**conjugate()** : 켤레복소수를 계산하여 반환합니다. (메서드/Method)
- **abs**(complex) : 복소수의 **크기(절댓값)**를 계산하여 반환합니다. (내장 함수/Built-in Function)

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
a=1+2j

print(a.real)
print(a.imag)
print(a.conjugate())
print(abs(a))
```

직접 `3 + 4j`라고 입력하는 방법 외에도, `complex(실수부, 허수부)` 함수를 사용하여 복소수를 만들 수 있습니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
a = 3+4j
b = 3
c = 4
z = complex(b, c)  # 3 + 4j 생성
print(a)
print(z)
```

---
## **math** 모듈 
<br>
파이썬은 `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `exp`, `log`, `sqrt`와 같은 다양한 기본 수학 함수들을 제공하며, 이들은 **math**라는 이름의 모듈에 저장되어 있습니다.

이 함수들을 사용하려면 반드시 해당 모듈을 **import**해야 합니다!

{: highlight}
`math` 모듈의 함수를 사용할 때는 반드시 앞에 `math.`을 붙여주어야 합니다.

```python
import math

a=math.sqrt(2)
b=math.sin(math.pi/2)
c=math.exp(3/4)
d=math.log(10)
e=math.log10(10)

print(a)
print(b)
print(c)
print(d)
print(e)
```

{: .warning }
`sin`, `cos` 등의 삼각함수는 '도(degree)'가 아닌 '라디안(radian)' 값을 인자로 받습니다.
내장 함수 math.radians()을 사용하면 '도'를 '라디안'으로 변환할 수 있습니다.

---
## **산술 연산자 (Arithmetic Operators)**

|**연산자**|**이름**|**예시**|**설명**|
|:-------|:-------|:-------|:-------|
|+|더하기 (Addition) |x + y||
|-|빼기 (Subtraction)|x - y||
|*|곱하기 (Multiplication)| x * y ||
|/|나누기 (Division)|x / y |결과는 항상 실수|
|%|나머지 (Modulus)|x % y |나눗셈 후의 나머지를 반환|
|//|몫 (Floor division)|x // y|나눗셈 결과에서 소수점 이하를 버린 몫을 반환|
|**|거듭제곱 (Exponentiation)|x ** y|x의 y승을 계산|

{: .note }
> **`/`와 `//`의 차이**
>
> 파이썬에서 `5 / 2`는 `2.5`(실수)가 되지만, `5 // 2`는 소수점을 버린 `2`(정수)가 됩니다.

{: .note }
> **`%` 연산자의 활용**
>
> 홀수와 짝수를 구분하거나(`x % 2`), 특정 숫자의 배수인지 확인할 때 자주 사용됩니다.


---
## **할당 연산자 (Assignment Operators)**

|**연산자**|**예시**|**의미**|
|:-------|:-------|:-------|
|=        |x = 1     |x = 1    |
|+=       |x += 2    |x = x+2  |
|-=       |x -= 2    |x = x-2  |
|*=       |x *= 2    |x = x*2  |
|/=       |x / 2     |x = x/2  |
|%=       |x %= 3    |x = x%3  |
|//=       |x //= 3    |x = x//3  |
|**=       |x **= 3    |x = x**3  |

-**업데이트의 개념**

`x += 1`은 프로그래밍에서 매우 자주 쓰이는 패턴입니다. 이를 **"변수의 값을 업데이트한다"**라고 표현하며, 카운트를 세거나 합계를 누적할 때 핵심적인 역할을 합니다.

-**코드의 간결성**

`x = x + 3`보다 `x += 3`이 더 짧고 타이핑하기 편할 뿐만 아니라, 나중에 변수 이름이 길어질 경우(예: total_score_sum += score) 실수를 줄여줍니다. 

{: .warning }
> **⚠️ 주의**
> 대입 연산자를 사용하기 전에 반드시 변수가 먼저 정의되어 있어야 합니다.

```python
# 에러 발생 예시
y += 5  # y가 정의되지 않았으므로 NameError 발생
```

---

## **✍️ 연습 문제**