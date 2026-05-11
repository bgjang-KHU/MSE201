---
layout: default
title: 파일 입출력
parent: Ch5. 파일 입출력
nav_order: 1
---

# **파일 입출력 (File I/O)**

---

우리는 지금까지 값을 '입력'받을 때는 사용자가 직접 키보드로 입력하는 방식을 사용했고, '출력'할 때는 모니터 화면에 결과를 보여주는 방식을 사용했습니다. 하지만 실험 데이터처럼 방대한 양의 정보를 다루거나, 프로그램의 실행 결과를 저장하여 다른 사람과 공유하기 위해서는 **파일(File)**을 통한 입출력이 필수적입니다.  

이번 시간에는 가장 기본적인 데이터 저장 형태인 **텍스트 파일(.txt)**을 읽고 쓰는 방법을 배워보겠습니다.
일반적으로 `.txt` 확장자를 가진 텍스트 파일은 순수한 글자 데이터(plain text)만을 담고 있습니다. 
하지만 우리가 작성하는 프로그램이나 파일을 읽는 도구들은 대개 이 데이터가 **특정한 형식(Format)**에 따라 체계적으로 정리되어 있습니다.
예를 들어, 실험 장비에서 나온 데이터가 `.txt` 파일이라면, 단순히 글자가 들어있는 것이 아니라 "첫 번째 열은 시간, 두 번째 열은 온도"와 같은 특정한 형식을 가지고 있는 것입니다.

---


## **파일 생성해보기**
먼저 Python을 통해 실제로 파일을 하나 만들어 봅시다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
f = open("newfile.txt", 'w')
f.close()
```

프로그램을 실행한 디렉터리에 'newfile.txt'라는 새로운 파일이 생성된 것을 확인할 수 있습니다.

{: .warning }
파일이 어디에 생겼는지 내가 어디서 작업하고 있는지 잘 모르겠다면 저장하기를 통해 원하는 위치 (폴더)에 현재 작성 중인 Python 파일을 저장합니다. 원하는 위치에 저장한 후 다시 실행을 하면 해당 폴더에 새로운 파일이 생기는 것을 확인할 수 있습니다.


우리는 파일을 생성하기 위해 파이썬 내장 함수인 open을 사용했습니다. open 함수는 다음과 같이 '파일 이름'과 '파일 열기 모드'를 입력값으로 받고, 결과값으로 파일 객체를 반환합니다.

---

## **🛠️ `open` 함수의 기본 형식**

파일을 다루기 위해서는 가장 먼저 open 함수로 파일 객체를 생성해야 합니다.

```python
f = open(filename, mode) # 파일_객체 = open(파일_이름, 파일_열기_모드)
```

여기서 `f`는 파일 객체를 의미하며, `filename`은 열고자 하는 파일의 이름 (혹은 경로), `mode`는 파일을 어떤 용도로 사용할지를 결정하는 문자열입니다.

### 🛠️ **파일 열기 모드** 
대표적으로 사용하는 3가지 모드는 다음과 같습니다.

|**모드**|**설명**|
|:-------|:-------|
|`r`| 읽기 모드 (**r**ead): 파일을 읽기만 할 때 사용합니다. (기본값)  | 
|`w`| 쓰기 모드 (**w**rite): 파일에 내용을 쓸 때 사용합니다. |
|`a`| 추가 모드 (**a**ppend): 파일의 마지막에 새로운 내용을 추가할 때 사용합니다. | 

{: .warning }
쓰기 모드(`w`)로 파일을 열 때, 만약 같은 이름의 파일이 이미 존재한다면 기존의 내용은 모두 사라지니 주의해야 합니다.

---

## **파일 쓰기**

이제 파일에 실제 내용을 기록해 봅시다. 모니터에 결과를 출력할 때 `print()`를 사용했던 것처럼, 파일에 기록할 때는 파일 객체의 `write()` 함수를 사용합니다.

### **🚀 TRY IT!**
{: .text-blue-200 }
test.txt 파일을 생성하고 0번부터 4번 줄까지 내용을 적어봅시다.

```python
f = open('test.txt', 'w')
for i in range(5):
    f.write(f"This is line {i}\n") # 끝에 \n을 붙여야 다음 줄로 넘어갑니다.
f.close()
```

> {: .highlight }
> This is line 0  
> This is line 1  
> This is line 2  
> This is line 3  
> This is line 4


{: .warning }
`write()` 함수는 `print()`와 달리 자동으로 줄을 바꿔주지 않습니다. 따라서 줄을 바꾸고 싶다면 반드시 문자열 끝에 줄바꿈 문자(`\n`)를 직접 넣어줘야 합니다.

### **🚀 TRY IT!**
{: .text-blue-200 }
```python
f = open('without_newline.txt', 'w')
for i in range(5):
    f.write(f"This is line {i}") 
f.close()
```

> {: .file }
> This is line 0This is line 1This is line 2This is line 3This is line 4

### **파일 내용 추가하기**

쓰기 모드(`w`)로 파일을 열면 기존에 있던 내용이 모두 사라지고 새로 작성됩니다. 만약 기존 내용을 유지하면서 뒤에 새로운 내용을 덧붙이고 싶다면 추가 모드(`a`)를 사용해야 합니다.

### **🚀 TRY IT!**
{: .text-blue-200 }
기존 test.txt 파일 뒤에 5번부터 9번 줄까지 내용을 추가해 봅시다.

```python
f = open('test.txt', 'a') # 추가 모드('a')로 열기
for i in range(5, 10):
    f.write(f"This is line {i}\n")
f.close()
```

프로그램을 실행한 뒤 test.txt 파일을 열어보면, 기존의 0~4번 줄 뒤에 5~9번 줄이 이어서 저장된 것을 확인할 수 있습니다.

### **⚠️ 중요: 파일을 반드시 닫아야 하는 이유 (f.close)**

파일 작업이 끝나면 항상 `f.close()`를 사용하여 파일을 직접 닫아주는 것이 좋습니다.

- **데이터 손실 방지**: 파일을 쓰기 모드로 열었을 때, 데이터를 기록하더라도 `close()`를 하기 전까지는 실제 디스크에 저장되지 않고 메모리에 머물러 있을 수 있습니다.
- **리소스 해제**: 파일을 열어둔 채로 방치하면 시스템의 자원을 계속 차지하게 되며, 때로는 다른 프로그램에서 해당 파일을 사용하지 못하게 막는 원인이 됩니다.

---

## **파일 읽기**

Python에서는 용도에 따라 파일을 읽는 여러 가지 함수를 제공합니다. 하나씩 살펴봅시다.

### **1. read(): 파일 전체를 하나의 문자열로 읽기**
`f.read()`는 파일의 내용 전체를 하나의 커다란 문자열로 반환합니다. 파일의 내용이 아주 크지 않을 때 한꺼번에 가져오기 편리합니다.

```python
f = open('test.txt', 'r')
content = f.read() # 전체를 하나의 string으로 읽음
print(content)
f.close()
```

> {: .result .fs-3 }
> This is line 0
> This is line 1
> This is line 2
> This is line 3
> This is line 4

이때 content의 type도 확인해봅시다! `read()`를 통해 읽어온 정보는 문자열로 저장됩니다.

```python
print(type(content))
```
> {: .result .fs-3 }
> > <class 'str'> 

<br>

### **2. readlines(): 모든 줄을 리스트에 담기**
`f.readlines()`는 파일의 모든 줄을 읽어서 각각의 줄을 요소로 가지는 리스트(List)를 반환합니다.

```python
f = open('test.txt', 'r')
content = f.readlines() # 전체를 하나의 string으로 읽음
print(content)
f.close()
```

> {: .result .fs-3 }
> ['This is line 0\n', 'This is line 1\n', 'This is line 2\n', 'This is line 3\n', 'This is line 4\n']

다음은 `for`문을 활용하여 `readlines()`로 읽은 정보를 출력해봅시다.

```python
f = open('test.txt', 'r')
lines = f.readlines() # ["1st line\n", "2nd line\n", ...]
for line in lines:
    print(line)
f.close()
```

> {: .result .fs-3 }
> > This is line 0  
> >  
> > This is line 1  
> >   
> > This is line 2  
> >  
> > This is line 3  
> > 
> > This is line 4
>  


### **💡 줄 바꿈(`\n`) 문자 제거하기 (strip)**
위의 `readlines()` 예제를 실행해 보면 줄 사이에 빈 줄이 하나씩 더 생기는 것을 볼 수 있습니다. 이는 파일의 각 줄 끝에 붙어 있는 줄 바꿈 문자(`\n`)와 `print()` 함수의 줄 바꿈 기능이 중복되기 때문입니다.
이때 `strip()` 함수를 사용하면 줄 끝의 줄 바꿈 문자를 깔끔하게 제거할 수 있습니다.


### **🚀 TRY IT!**
{: .text-blue-200 }

```python
f = open('test.txt', 'r')
while True:
    line = f.readline()
    if not line: break # 더 이상 읽을 줄이 없으면(빈 문자열) 반복 종료
    print(line.strip()) # strip()을 사용해 줄바꿈 제거
f.close()
```

파일에서 읽어온 데이터는 우리가 원하는 값 외에도 눈에 보이지 않는 공백이나 기호들이 섞여 있는 경우가 많습니다. 이때 `strip()`을 사용하면 데이터를 깨끗하게 다듬을 수 있습니다.

 **🛠️ 모든 공백 문자 제거하기**
 {: .text-blue-200 }

인자 없이 `strip()`을 쓰면 문자열 앞뒤의 공백(Space), 탭(\t), 줄 바꿈(\n)을 한 번에 날려줍니다.

```python
line = " \t 22.5 \n"
print(f"'{line.strip()}'")  # 출력: '22.5'
```

 **🛠️ 특정 문자 제거하기**
{: .text-blue-200 }

데이터 **끝**에 불필요한 기호(예: 콤마, 마침표)가 붙어 있을 때, 괄호 안에 해당 문자를 넣어 제거할 수 있습니다.

```python
# 센서 데이터 끝에 콤마가 붙어 있는 경우
data = ",,12.5, 30.2, 15.8,,"
clean_data = data.strip(",") # 마지막 콤마 제거
print(clean_data)  # 출력: 12.5, 30.2, 15.8
```
> {: .result .fs-3 }
> > 12.5, 30.2, 15.8 # 양 끝에 있던 콤마 제거됨


<br>


### **3. readline(): 한 줄씩 순차적으로 읽기**

`f.readline()`은 파일의 첫 번째 줄부터 한 줄씩 읽어옵니다. 만약 모든 줄을 읽고 싶다면 다음과 같이 `while` 문을 사용하여 무한 루프를 돌리다가, 더 이상 읽을 줄이 없을 때(`break`) 멈추게 작성합니다.

```python
f = open('test.txt', 'r')
while True:
    line = f.readline()
    if not line: break # 더 이상 읽을 줄이 없으면(빈 문자열) 반복 종료
    print(line.strip())
f.close()
```

<br>


### **4. 파일 객체를 for 문과 함께 사용하기**

파일 객체(`f`) 그 자체를 `for` 문에 넣으면, 파이썬이 알아서 파일을 줄 단위로 읽어줍니다. 코드가 가장 간결하고 가독성이 좋아 실무에서 가장 권장되는 방법입니다.

```python
# read_for.py
f = open('test.txt', 'r')
for line in f: # 파일 객체를 직접 반복문에 사용
    print(line.strip())
f.close()
```

{: .highlight }
>💡 **실무 팁 (TIP)**  
이 방법은 `readlines()`처럼 모든 데이터를 미리 메모리에 다 올리지 않고 한 줄씩 처리하기 때문에, 거대한 데이터를 다룰 때 메모리를 효율적으로 사용할 수 있다는 장점도 있습니다.

---

## **with 문과 함께 사용하기**

지금까지 우리는 파일을 열면(`open`) 항상 직접 닫아(`close`) 주어야 한다고 배웠습니다. 하지만 작업을 하다 보면 실수로 닫는 것을 깜빡할 수도 있고, 코드가 길어지면 번거롭기도 합니다. 파이썬의 `with` 문은 이러한 과정을 자동으로 처리해 주는 아주 편리한 기능입니다.

### **🛠️ 기본구조**

```python
with open("with.txt", "w") as f:
    f.write("Life is too short, you need python")
```

`with` 문을 사용하면 `with` 블록(들여쓰기 된 부분)을 벗어나는 순간, 열려 있던 파일 객체 `f`가 자동으로 닫힙니다. 따라서 별도로 `f.close()`를 작성할 필요가 없습니다.

### **🤔 정말로 닫혔는지 확인해 볼까요?**

파일 객체의 `closed` 속성을 사용하면 파일이 현재 닫혀 있는 상태인지(`True`), 열려 있는 상태인지(`False`) 확인할 수 있습니다.

```python
with open("with.txt", "w") as f:
    f.write("Hello")
    print(f"작업 중: {f.closed}") # False (아직 열려 있음)

print(f"작업 종료 후: {f.closed}") # True (자동으로 닫힘)
```

{: .highlight }
>💡 **실무 팁 (TIP)**  
실무에서는 예외 상황에서도 파일을 안전하게 보호하기 위해 open-close 방식보다는 with 문 사용을 권장합니다.