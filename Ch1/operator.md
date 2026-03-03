---
layout: default
title: 논리 표현식과 연산자
parent: Ch1. Python 기초
nav_order: 3
---

# 논리 표현식과 연산자

---

## 논리표현식 (Logical Expression)이란?

논리 표현식이란 결과가 참(True) 또는 거짓(False) 중 하나로 도출되는 문장을 말합니다.  
예를 들어, `a < b`는 `a`와 `b`에 어떤 값이 들어오느냐에 따라 참일 수도, 거짓일 수도 있는 논리 표현식입니다.

{: .highlight }
>💡 **수학 vs 프로그래밍의 차이**
>- 수학에서의 *a < b* : "a 는 b 보다 작다"라는 확정된 사실(명제)를 말합니다. 즉, a>=b인 상황은 허용되지 않습니다.
>- 프로그래밍에서의 `a < b` : Python에게 던지는 **질문**입니다. "지금 `a`가 `b`보다 작니?" 라고 묻는 것이며, Python은 상황에 따라 `True` 또는 `False`라고 답합니다.

## 불 (Boolean) 자료형

Python은 논리 표현식의 결과로 `True`와 `False`라는 값을 내놓습니다. 이를 불 (Boolean) 자료형이라고 부릅니다. 더 자세한 내용은 다음 챕터에서 다룰 예정입니다.

- True: 숫자 1과 같은 의미로 취급됩니다.
- False: 숫자 0과 같은 의미로 취급됩니다.

Python 내부적으로 `True`는 1, `False`는 0의 성질을 갖지만, 입문 단계에서는 논리적 상태를 나타내는 고유한 값으로 이해하는 것이 좋습니다.

## 비교 연산자 (Comparison Operators)

비교 연산자는 두 값을 비교하여 논리 표현식을 만드는데 사용됩니다. Python에서 사용하는 기호는 다음과 같습니다.

|**연산자**|**이름**|**예시**|
|:-------|:-------|:-------|
|==       |Equal                      |x == y    |
|!=       |Not equal                  |x != y  |
|>       |Greater than                |x > y  |
|<       |Less than                   |x < y  |
|>=       |Greater than or equal to   |x >= y  |
|<=       |Less than or equal to      |x <=> y  |

### **TRY IT!**
{: .text-blue-200 }

```python
print(5 == 4)
print(2 < 3)
```
> {: .result }
> > False
> >
> > True

## 논리 연산자 (Logical Operator)

논리 연산자는 두 개의 논리 표현식 (편의상 $P$와 $Q$라고 부릅니다) 사이의 관계를 정의하는 연산입니다. 주로 다룰 핵심 논리 연산자는 **and**, **or**, **not**입니다.

|**연산자**|**설명**|**예시**|**결과**|
|:-------|:-------|:-------|:-------|
|and     | 논리곱 (Both True) | P and Q | P와 Q가 모두 참일 때만 True, 아니면 False |
|or      | 논리합 (Either True) | P or Q | P와 Q 중 하나라도 참이면 True, 둘 다 거짓이면 False |
|not     | 논리 부정 (Reverse)  | not P  | P의 상태를 반대로 뒤집음 (True ↔ False) |