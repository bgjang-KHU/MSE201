---
layout: default
title: 숫자형
parent: Ch2. 변수와 기본 자료구조
nav_order: 2
---
# 숫자형 

---

|  항목   |   예시            |
|:-------|:------------------|
|Integer (int, 정수)              | 123, -345, 0        |
|Floating-point (float, 실수)     | 123.45, 1.0, 3.4e10 |
|Complex number (complex, 복소수) | 1+2j, -3J            |

---

### **정수형**

정수형(integer)은 말 그대로 정수를 뜻하는 자료형이다. 

```python
a=123
print(a)
print(type(a))
```
> {: .result .fs-4 }
> > 123
> > 
> > <class 'int'>

{: .tip }
내장 함수 `type`을 사용하여 자료의 유형을 확인할 수 있다.


---

### **실수형**

실수형(floating-point)은 소수점이 포함된 숫자를 말한다.

```python
b=3.4e10
print(b)
print(type(b))
```
> {: .result .fs-3 }
> > 34000000000.0
> > 
> > <class 'float'>

---

### **복소수형**

복소수형(complex)은 실수부와 허수부로 구성된 숫자를 말하며, 파이썬에서는 허수 단위로 $j$를 사용한다.

```python
c=1+2j
print(c)
print(type(c))
```
> {: .result }
> > (1+2j)
> > 
> > <class 'complex'>

---

`1`은 정수형(int)이고, `1.0`은 실수형(float)이다.
이들의 유형을 바꾸는 것도 가능하다.

### **TRY IT!**
{: .text-red-200 }

```python
a=1
print(a)
print(type(a))

b=float(a)
print(b)
print(type(b))
```


> {: .result .fs-4 }
> > 1
> > 
> > <class 'int'>
> >
> > 1.0
> >
> > <class 'float'>