---
layout: default
title: 딕셔너리 자료형
parent: Ch2. 변수와 기본 자료구조
nav_order: 7
---
# 딕셔너리 자료형 (Dictionaries)

---

딕셔너리는 이름 그대로 '사전'과 같은 구조를 가집니다. 단어(Key)를 찾으면 그 뜻(Value)이 나오듯이, **키와 값이 한 쌍**으로 묶여 있는 자료형입니다.
실무에서 가장 빈번하게 사용되는 자료형이기도 합니다.

## **📦 집합 (Sets) 만들기**
<br>
중괄호 `{ }`를 사용하며, 각 쌍은 `Key: Value` 형태로 표현하고 쉼표(`,`)로 구분합니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
# 과일 개수 저장
dict1 = {'apple': 3, 'orange': 4, 'pear': 2}

# 학교와 국가 매칭 (KHU 예시)
dict2 = {
    "KHU": "South Korea",
    "Oxford": "UK",
    "UC Berkeley": "USA"
}
```
---

{: .warning } 
> 1. Key는 중복될 수 없습니다: 만약 동일한 Key를 두 번 쓰면, 나중에 쓴 값으로 덮어씌워집니다.
> 2. Key에 리스트는 쓸 수 없습니다: Key는 값이 변하지 않는(Immutable) 데이터여야 합니다. 따라서 리스트는 안 되지만, 튜플은 Key로 사용할 수 있습니다. (Value에는 리스트든 뭐든 다 들어갈 수 있습니다!)


## **🔍 데이터 꺼내기: 인덱스 번호 대신 'Key'**

딕셔너리는 리스트처럼 `[0]`, `[1]` 같은 인덱스를 쓰지 않습니다. 대신 설정한 **Key**를 사용하여 값을 찾습니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
dict2 = {
    "KHU": "South Korea",
    "Oxford": "UK",
    "UC Berkeley": "USA"
}

print(dict2['KHU'])  # 'South Korea' 출력

# 데이터 추가 및 수정
dict2['Harvard'] = 'USA'
print(dict2)
```

---

## **🛠️ 딕셔너리 주요 메서드**

- `.keys()`: 딕셔너리의 모든 Key만 모아서 보여줌
- `.values()`: 딕셔너리의 모든 Value만 모아서 보여줌
- `.items()`: Key와 Value의 쌍을 튜플 형태로 모두 보여줌
- `.get(key)`: Key에 해당하는 값을 가져옴 (없는 Key일 때 에러 대신 None 반환)
- `in`: 특정 Key가 딕셔너리 안에 있는지 확인 (`True/False`)

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
a = {'name': 'pey', 'phone': '010-1234-5678'}

print('name' in a)       # True
print(a.get('email'))    # None (에러가 나지 않아 안전함)
print(list(a.keys()))    # ['name', 'phone']
```

### **🤔 Wait and Think!**
`a['email']`과 `a.get('email')`의 차이는 무엇일까요?  
- `a['email']`: Key가 없으면 프로그램을 즉시 중단시키며 에러를 냅니다.
- `a.get('email')`: Key가 없어도 에러 대신 **None**을 돌려주며 프로그램을 계속 실행합니다.

{: .highlight }
>💡 **실전 팁 (TIP)**    
> 데이터가 있을지 없을지 불확실한 상황에서는 프로그램이 갑자기 멈추지 않도록 **.get()**을 사용하는 것이 훨씬 프로다운 코딩입니다.