---
layout: default
title: 함수의 기본
parent: Ch3. 함수
nav_order: 1
---

# 함수의 기본 (Functions Basics)

---

## **함수의 정의 (Defining a Function)**

 Python에서 나만의 함수를 만드는 가장 일반적인 방법은 `def` 키워드를 사용하는 것입니다. 함수는 크게 정보를 받는 **머리(Header)**와 실제 일을 하는 **몸통(Body)**으로 나뉩니다.

🛠️ 함수의 기본 구조

```python
def 함수이름(매개변수1, 매개변수2, ...): # (1) 함수 헤더
    '''
    함수에 대한 설명 (Descriptive String)
    '''
    # (2) 함수 몸통 (수행할 문장들)
    결과 = 매개변수1 + 매개변수2
    
    return 결과 # (3) 반환값 (선택 사항)
```