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
- `.items()`: Key와 Value의 쌍을 **튜플** 형태로 모두 보여줌
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

---

## **🛠️ 딕셔너리 메서드 실습: 신소재 물성 데이터 관리**

 주요 금속의 융점(°C) 데이터를 딕셔너리로 관리해 봅시다.

{: .highlight }
> - **재료의 'ID'는 Key!**: 금속 이름은 고유하니까 Key로 쓰고, 그 성질인 융점은 Value로 둡니다. 만약 금속 이름을 중복해서 쓰면 최신 값으로 덮어씌워지니 주의해야 합니다.
> - **물성치 검색의 효율성**: 수천 개의 데이터를 다룰 때, 리스트에서 하나하나 찾는 것보다 딕셔너리의 Key로 찾는 게 훨씬 효율적입니다.

 ```python
# 금속 재료 데이터베이스 (Key: 금속 이름, Value: 융점)
metal_melting_points = {
    "Iron": 1538,
    "Gold": 1064,
    "Aluminum": 660,
    "Copper": 1085,
    "Titanium": 1668
}
```

### **1. 데이터 전체 확인 (`.keys()`, `.values()`, `.items()`)**
현재 데이터베이스에 어떤 금속들이 있고, 그 수치들이 어떻게 되는지 한꺼번에 확인하는 방법입니다. 위의 코드에 이어 붙여서 확인해봅시다.  
(아직 배우지 않았지만 `for` 문에 대해서 생각해봅시다! 들여쓰기에 유의합니다.)

 ```python
# (1) .keys() : 현재 등록된 금속 종류 확인
metals = metal_melting_points.keys()
print(f"🔬 분석 대상 금속: {list(metals)}")

# (2) .values() : 모든 금속의 융점 수치만 따로 뽑기
points = metal_melting_points.values()
print(f"🔥 등록된 융점 데이터: {list(points)}")

# (3) .items() : 금속과 융점을 짝지어 보고서 형태로 출력
print("--- [금속별 융점 보고서] ---")
for metal, mp in metal_melting_points.items():
    print(f"{metal}의 융점은 {mp}°C 입니다.")
```

### **🤔 Wait and Think!**
`.items()`으로 받아온 정보를 언패킹을 통해 `metal` 과 `mp` 라는 변수에 할당하고 있는 것을 확인했나요?  
다음의 경우 어떻게 출력되는지 확인해볼까요?

 ```python
for item in metal_melting_points.items():
    print(item)  # ('Iron', 1538) <- 튜플이 통째로 출력됨
```


### **2. 특정 데이터 안전하게 찾기 (`.get()`)**
특정 재료의 물성치를 검색할 때 사용합니다. 만약 데이터베이스에 없는 금속을 검색하더라도 프로그램이 "에러"를 내며 꺼지지 않게 하는 것이 포인트입니다.

 ```python
# (4) .get() : 안전한 호출
# 존재하는 금속 검색
temp = metal_melting_points.get("Iron")
print(f"철(Iron)의 융점: {temp}°C")

# 존재하지 않는 금속 검색 (에러 대신 "데이터 없음" 출력)
temp_unknown = metal_melting_points.get("Silver", "데이터 없음")
print(f"은(Silver)의 융점: {temp_unknown}")
```

### **3. 데이터 존재 여부 확인 (`in`)**
우리가 가진 데이터베이스에 해당 정보가 있는지 미리 확인할 때 유용합니다.  
(아직 배우지 않았지만 `if`, `else` 문에 대해서 생각해봅시다! 들여쓰기에 유의합니다.)

 ```python
# (5) in : 포함 여부 확인
target_metal = "Titanium"

if target_metal in metal_melting_points:
    print(f"✅ {target_metal}은(는) 분석 가능한 재료입니다.")
else:
    print(f"❌ {target_metal}에 대한 정보가 없습니다.")
```

### **4. 불필요한 데이터 제거 (`.pop()`)**

 ```python
# (6) .pop() : 특정 항목 삭제 및 값 반환
# 구리(Copper) 데이터를 지우면서 융점 값을 변수에 저장
removed_mp = metal_melting_points.pop("Copper")

print(f"🗑️ 삭제된 항목(Copper)의 융점: {removed_mp}°C")
print(f"📋 업데이트된 데이터베이스: {metal_melting_points}")
```