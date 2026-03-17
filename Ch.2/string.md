---
layout: default
title: 문자열 자료형
parent: Ch2. 변수와 기본 자료구조
nav_order: 3
---
# 문자열 자료형 (String)

---

문자열 (string)은 연속된 문자들의 나열입니다. 다음은 문자열의 예시입니다.

```python
"Life is too short, You need Python"
"a"
"123"
```

따옴표로 둘러싸여 있으면 모두 문자열입니다. `123`과 `"123"`은 각각 정수형과 문자열로 취급됩니다.

{: .warning }
`"123"`은 숫자형이 아닌 문자열로 취급됩니다.


---

## 문자열 만들기

Python에서 문자열을 만드는 방법은 4가지입니다. 큰 따옴표 (" ")나 작은 따옴표 (' ')로 양쪽으로 둘러쌓여 있거나, 큰 따옴표 3개 ("""  """) 혹은 작은 따옴표 3개 ('''  ''')로 둘러쌓여 있으면 문자열로 취급됩니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

다음 문자열들을 출력해보고, type() 함수를 확인해 자료의 유형을 직접 확인해보세요.

```python
a = "S"
b = 'Hello World'
c = """   """
d = '''I love Python!'''
```

{: .highlight }
`c`의 출력값은 어떤가요? 데이터의 유형은요?  
우리 눈에 보이지 않는다고 해서 데이터가 없는 것은 아닙니다. 따옴표에 둘러쌓인 공백도 문자열 (string)으로 취급됩니다.


### **🤔 Wait and Think!**

그렇다면 왜 다양한 방식으로 문자열을 정의하도록 했을까요? 그리고 어떤 점이 다를까요?

1. 아래 코드를 실행해봅시다. `"""` 를 `"`로 바꾸면 어떻게 될까요?

```python
long_string = """To. Students
Happy Coding!
From. Professor
"""
print(long_string)
```

2. 아래 코드를 실행해봅시다. 만약 양쪽 끝의 `"` 를 `'`로 바꾸면 어떻게 될까요?

```python
quote = "He said, 'Python is easy.'"
print(quote)
```

{: .highlight }
>- 일반 따옴표 한 개를 쓰면 줄바꿈을 할 때 에러가 나지만, 3개짜리를 쓰면 엔터 키를 친 그대로 저장됩니다.
>- 문자열 안에 작은따옴표(`'`)를 넣고 싶다면 바깥을 큰따옴표(`"`)로 감싸면 됩니다. 반대의 경우도 마찬가지입니다.

---

## 이스케이프 코드 (Escape code)

역슬래시(`\`)는 Python에게 **"바로 뒤에 오는 문자를 평소와 다르게 해석해줘"** 라고 요청합니다. 
역슬래시(`\`)를 사용하면 특정 문자의 원래 기능을 **'탈출(Escape)'**시켜 새로운 기능을 부여할 수 있습니다.

자주 사용하는 이스케이프 코드는 다음과 같습니다.

|**코드**|**설명**|
|:-------|:-------|
|`\'`      | 작은 따옴표 (')를 그대로 표현 |
|`\"`      | 큰 따옴표 (")를 그대로 표현 |
|`\n`      | 문자열 안에서 줄을 바꿈  |
|`\t`      | 문자열 사이에 탭 간격을 줌 |
|`\\`      | `\`를 그대로 표현 |

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
print('Python\'s favorite food is perl')
print('\\ is slash')
print('Change line\nwith "\\n"')
print('Tab\twith "\\t"')
```

> {: .result .fs-3 }
> > Python's favorite food is perl  
> > \ is slash  
> > Change line  
> > with "\n"  
> > Tab    with "\t"

---


## 문자열 연산

Python에서는 문자열을 더하거나 곱할 수 있습니다. 다른 언어에서는 찾아보기 어려운 기능입니다.

### 문자열 더하기

```python
a = "Hello,"
b = " MSE!"
print(a+b)
```
> {: .result .fs-3 }
Hello, MSE!

### 문자열 곱하기

```python
a = "Hello "
print(5*a)
```
> {: .result .fs-3 }
Hello Hello Hello Hello Hello

---

## 문자열 길이 구하기

`len()` 함수는 문자열의 길이(문자 개수)를 반환하는 내장 함수입니다.
```python
a = "Hello World"
l = len(a)
print(l)
```
> {: .result .fs-3 }
11

`"Hello World"`는 공백 포함 총 11개의 문자로 이루어져 있습니다.

{: .highlight }
공백(space)도 하나의 문자로 취급됩니다.

---

## 문자열 인덱싱과 슬라이싱

### 인덱싱 (Indexing)

문자열의 각 문자에는 **인덱스(index)**라는 위치 번호가 부여됩니다. 대괄호 `[ ]`와 인덱스를 사용하면 특정 위치의 문자에 접근할 수 있습니다.

{: .warning }
**인덱스는 0부터 시작합니다!** 첫 번째 문자의 인덱스는 `1`이 아닌 `0`입니다.

`"Hello World"`의 인덱스 구조는 다음과 같습니다.

![](https://bgjang-khu.github.io/MSE201/assets/images/index.png)


음수 인덱스를 사용하면 문자열의 **뒤에서부터** 접근할 수 있습니다. `-1`은 마지막 문자, `-2`는 끝에서 두 번째 문자를 의미합니다.

### **🚀 TRY IT!**
{: .text-blue-200 }
```python
a = "Hello World"
print(a[5])
print(a[6])
print(a[-1])
```

> {: .result .fs-3 }
> > &nbsp;  
> > W  
> > d

{: .highlight }
`a[5]`의 출력값은 공백(` `)입니다. 공백도 인덱스를 가진 문자입니다.

---

### 슬라이싱 (Slicing)

슬라이싱을 사용하면 문자열의 **일부분**을 잘라낼 수 있습니다. 형식은 `[start:end:step]`이며, `end` 인덱스의 문자는 **포함되지 않습니다**.

| 요소 | 의미 | 기본값 |
|:-----|:-----|:------|
| `start` | 시작 인덱스 | 0 (생략 시 처음부터) |
| `end` | 끝 인덱스 (미포함) | 끝 (생략 시 끝까지) |
| `step` | 몇 칸씩 건너뛸지 | 1 (생략 가능) |


### **🚀 TRY IT!**
{: .text-blue-200 }
```python
a = "Hello World"

print(a[6:11])  # 인덱스 6~10
print(a[6:])    # 인덱스 6부터 끝까지
print(a[:5])    # 처음부터 인덱스 4까지
print(a[6:-2])  # 인덱스 6부터 끝에서 두 번째 전까지
print(a[::2])   # 한 칸씩 건너뛰며 전체 슬라이싱
```

> {: .result .fs-3 }
> > World  
> > World  
> > Hello  
> > Wor  
> > HloWrd

### 슬라이싱으로 문자열 나누기

슬라이싱을 활용하면 문자열 안에 포함된 정보를 손쉽게 추출할 수 있습니다.
```python
a = "20260317Rainy"
year    = a[:4]
day     = a[4:8]
weather = a[8:]
print('Year=', year, 'Date=', day, 'Weather=', weather)
```

> {: .result .fs-3 }
> > Year= 2024 Date= 0312 Weather= Rainy

{: .highlight }
날짜, 코드번호, 센서 데이터처럼 **규칙적인 형식**으로 저장된 문자열에서 슬라이싱은 매우 유용합니다.

---

## 문자열 포매팅

포매팅(Formatting)은 **변수 값을 문자열 안에 삽입**하는 방법입니다. Python에는 4가지 포매팅 방식이 있습니다.

### 1. 문자열 연결 (`+` 연산자)

`+` 연산자로 문자열을 이어 붙여 포매팅할 수 있습니다. 단, 숫자형 변수는 `str()`로 먼저 변환해야 합니다.
```python
name = "John"
age = 30
formatted_string = "My name is " + name + " and I am " + str(age) + " years old."
print(formatted_string)
```

> {: .result .fs-3 }
My name is John and I am 30 years old.

{: .warning }
변수가 많아질수록 코드가 복잡해지고 실수하기 쉽습니다. 변수가 여러 개일 때는 아래 방식을 사용하는 것이 좋습니다.

---

### 2. `%` 포매팅

문자열 안에 `%s`, `%d` 등의 **자리표시자(placeholder)**를 넣고, `%` 연산자로 값을 채워 넣는 방식입니다.

| 코드 | 설명 |
|:-----|:-----|
| `%s` | 문자열 (String) |
| `%c` | 문자 하나 (Character) |
| `%d` | 정수 (Integer) |
| `%f` | 실수 (Float) |

```python
name = "John"
age = 30
formatted_string = "My name is %s and I am %d years old." % (name, age)
print(formatted_string)
```

> {: .result .fs-3 }
My name is John and I am 30 years old.

---

### 3. `str.format()` 메서드

중괄호 `{}`를 자리표시자로 사용하고 `.format()`에 값을 전달합니다. `{}`안에 숫자를 넣어 순서를 지정할 수도 있습니다.

### **🚀 TRY IT!**
{: .text-blue-200 }
```python
name = "John"
age = 30

# 순서대로 채우기
formatted_string = "My name is {} and I am {} years old.".format(name, age)
print(formatted_string)

# 인덱스로 순서 지정
fstring2 = "My name is {1} and I am {0} years old.".format(name, age)
print(fstring2)
```

> {: .result .fs-3 }
> > My name is John and I am 30 years old.  
> > My name is 30 and I am John years old.

{: .highlight }
`{0}`, `{1}` 같은 인덱스를 사용하면 값을 원하는 순서로 배치하거나 같은 값을 여러 번 재사용할 수 있습니다.

---

### 4. f-string (Python 3.6+)

문자열 앞에 `f`를 붙이고, 중괄호 `{}` 안에 **변수나 식을 직접** 넣는 방식입니다. 가장 간결하고 직관적이어서 현재 가장 널리 사용됩니다.
```python
name = "John"
age = 30
formatted_string = f"My name is {name} and I am {age} years old."
print(formatted_string)
```

> {: .result .fs-3 }
My name is John and I am 30 years old.

---

## 숫자 포매팅 (Format code with number)

포매팅 방식에 숫자 옵션을 추가하면 **정렬, 자릿수, 소수점 자리** 등을 세밀하게 제어할 수 있습니다.

### 정렬과 공백

| 지정자 | 의미 |
|:-------|:-----|
| `%10s` / `{:>10}` | 오른쪽 정렬, 전체 너비 10 |
| `%-10s` / `{:<10}` | 왼쪽 정렬, 전체 너비 10 |
| `{:^20}` | 가운데 정렬, 전체 너비 20 |

### **🚀 TRY IT!**
{: .text-blue-200 }
```python
print("'%10s'" % 'Hi')          # 오른쪽 정렬
print("'%-10s'" % 'MSE201')       # 왼쪽 정렬

print("'{:<20}'".format('Hi'))       # 왼쪽 정렬
print("'{:>20}'".format('MSE'))     # 오른쪽 정렬
print("'{:^20}'".format('Friends'))  # 가운데 정렬

name = "John"
print(f"My name is '{name:^20}'.")
```

> {: .result .fs-3 }
> > '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;hi'  
> > 'AMIE&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'  
> > 'Hi&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'  
> > '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AMIE'  
> > '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Friends&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'  
> > My name is '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;John&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'.



### 실수 소수점 자리 지정

{: .highlight }
`%전체너비.소수점자리f` 또는 `{:전체너비.소수점자리f}` 형식으로 실수의 출력 형태를 지정합니다.

### **🚀 TRY IT!**
{: .text-blue-200 }
```python
import math
pi = math.pi

print("'%15.2f'" % pi)              # 전체 15자리, 소수점 2자리
print("'{:15.6f}'".format(pi))      # 전체 15자리, 소수점 6자리
print(f"'{pi:15.4f}'")              # 전체 15자리, 소수점 4자리
```

> {: .result .fs-3 }
> > '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.14'  
> > '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.141593'  
> > '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.1416'

### 공백을 다른 문자로 채우기

`{:채울문자<너비}` 형식으로 공백 대신 원하는 문자를 채울 수 있습니다.
```python
import math
pi = math.pi

print("'{:=<15.6f}'".format(pi))   # 오른쪽 공백을 '='로 채움
print(f"'{pi:a>15.4f}'")           # 왼쪽 공백을 'a'로 채움
```

> {: .result .fs-3 }
> > '3.141593======='  
> > 'aaaaaaaaa3.1416'

---

## ✂️ 문자열 나누기: split() 함수 

`split()` 함수는 긴 문자열을 특정 기준에 따라 조각조각 나누어 주는 아주 유용한 도구입니다.

### 1. 공백을 기준으로 나누기 

`string.split()`처럼 괄호 안에 아무것도 넣지 않으면 **공백(Space), 탭(Tab), 엔터(Enter)**를 기준으로 문자열을 자동으로 나누어 줍니다.

```python
string = "Life is too short"
print(string.split())
```

> {: .result .fs-3 }
['Life', 'is', 'too', 'short']

### 2. 특정 문자를 기준으로 나누기

괄호 안에 특정 값을 넣어주면 그 값을 **구분자(Separator)**로 삼아 문자열을 나눕니다. 데이터가 쉼표(,)나 콜론(:)으로 연결되어 있을 때 유용합니다.

```python
string  = "a:b:c:d"
print(string .split(':'))
```

> {: .result .fs-3 }
['a', 'b', 'c', 'd']

### **🤔 Wait and Think!**

결과값에 대괄호 [ ]가 붙어 있어요! 이건 뭔가요?  
split()을 사용한 결과는 **리스트(List)**라는 자료구조에 하나씩 담기게 됩니다. ['Life', 'is', 'too', 'short']처럼 조각들이 순서대로 담긴 주머니라고 생각하면 됩니다.  
리스트 자료형에서 더 자세히 배울 예정입니다.

{: .highlight }
>💡 **실무 팁 (TIP)**   
엑셀 파일(CSV)이나 센서 데이터는 보통 12.5, 30.2, 15.8처럼 쉼표로 연결되어 들어오는 경우가 많습니다. 이때 split(',')을 사용하면 각 숫자 데이터를 손쉽게 분리해낼 수 있습니다.

### **🤔 Wait and Think!**

그렇다면 리스트에 담긴 나눈 문자열에서 필요한 것만 어떻게 뽑아낼까요?

`split()`의 결과물인 **리스트**도 문자열과 마찬가지로 **인덱스(Index)**를 사용합니다. 대괄호 [ ] 안에 번호를 넣어 원하는 조각만 쏙 골라낼 수 있습니다.

### **🚀 TRY IT!** 이메일 주소에서 도메인만 추출하기

이메일 주소는 항상 ID@domain.com 형식을 가집니다. 여기서 서비스 제공업체(도메인) 정보만 뽑아볼까요?

```python
email = "mse201@khu.ac.kr"

# 1. '@' 기호를 기준으로 나눕니다.
parts = email.split('@')
print(parts) 

# 2. 리스트의 두 번째 요소(인덱스 1)를 호출합니다.
domain = parts[1]
print("도메인 정보:", domain)
```

### **🚀 TRY IT!** 여러 데이터 중 하나만 골라 쓰기

다음과 같이 센서에서 들어온 데이터 중 '온도' 값만 필요할 때도 같은 원리를 사용합니다.

```python
# 날짜, 시간, 온도, 습도 데이터가 쉼표로 연결됨
data = "2026-03-18,10:30,22.5,45%"

# 쉼표(,)로 나누기
parsed_data = data.split(',')

# 인덱스 2번(세 번째 요소)이 온도 데이터입니다.
temp = parsed_data[2]
print(f"현재 온도는 {temp}도 입니다.")
```

✅ 요약하자면!

1. **split()**은 문자열을 조각내어 **리스트(List)**라는 주머니에 넣습니다.  

2. 주머니 안의 조각들은 0번부터 시작하는 인덱스 번호를 가집니다.  

3. 리스트명[인덱스] 형식을 사용하면 원하는 조각만 바로 꺼내어 쓸 수 있습니다.

{: .highlight }
>💡 **실무 팁 (TIP)**   
문자열 슬라이싱(`a[0:5]`)과 리스트 인덱싱(`parts[1]`)은 모두 **"데이터의 위치(번호)를 이용한다"**는 점에서 원리가 같습니다. 이 번호 매기기 규칙만 잘 이해하면 파이썬의 모든 자료구조를 자유자재로 다룰 수 있습니다!