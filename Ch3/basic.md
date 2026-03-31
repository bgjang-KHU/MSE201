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

### **🛠️ 함수의 기본 구조**

```python
def 함수이름(매개변수1, 매개변수2, ...): # (1) Function Header
    '''
    함수에 대한 설명 (Descriptive String)
    '''
    # (2) Function Body (수행할 문장들)
    Function statement
    
    return 결과 # (3) 반환값 (선택 사항)
```

### **🔍 함수의 핵심 구성 요소**

1. Function Header  
- 반드시 `def` 키워드로 시작합니다.
- 함수 이름 뒤에는 소괄호 `()`와 매개변수 (입력값)을 적습니다.
- 문장의 끝에는 반드시 **콜론(`:`)** 붙입니다.

2. Function Body
- 헤더 아래 **들여쓰기 (Indentation)**된 블록을 의미합니다. (보통 공백 4칸을 권장합니다.)
- **설명 문자열 (Descriptive string)**: 함수가 무슨 일을 하는지 적어두는 곳입니다. 없어도 무방하지만 나중에 `help(함수이름)` 이 명령어로 내용을 확인할 수 있어 다른 사람들과 협업에는 필수적입니다. 
- **수행 문장 (Function statement)**: 함수가 호출되었을 때 실행되는 단계별 지시 사항입니다. `#`으로 시작하는 주석은 실행되지 않으며 코드 이해를 돕기 위해 작성합니다.

3. 