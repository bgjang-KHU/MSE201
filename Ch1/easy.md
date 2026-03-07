---
layout: default
title: Python 코딩 맛보기
parent: Ch1. Python 기초
nav_order: 3
---

# 🚀 Python 코딩 맛보기
<br>
아직 파이썬의 문법을 정식으로 배우지는 않았지만, 몇 가지 간단한 코드를 직접 실행해보며 감을 잡아봅시다!

## 1. Hello World!

우리들의 첫번째 코드입니다. 아래 코드를 실행해보세요.  

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
'Hello World!'
print('Hello World!')
```
> {: .result }
> > Hello World!

### **🤔 Wait and Think!**
{: .text-red-200 }

두 줄 모두 'Hello World!'라는 글자를 가지고 있는데 누가 출력이 된 것일까요?

- 첫 번째 줄 ("Hello World")은 값이 존재하지만, 화면에 출력하라는 명령이 없습니다.
- 두 번째 줄은 Python의 `print()`**함수**를 사용해 우리가 볼 수 있도록 결과를 화면에 출력합니다.

{: .highlight }
>💡 **가장 먼저 배우지만, 가장 중요한 함수 print()**  
> - Python에게 어떤 결과를 보여달라고 명령할 때는 반드시 `print()`라는 상자에 담아서 전달해야 합니다. 이것이 우리와 컴퓨터가 소통하는 가장 기본적인 방법입니다.
> - `print()` 함수는 **debug의 기본** 입니다. 코드 중간중간 `print()`함수를 사용해 어떻게 진행이 되고 있는지 살펴봅시다! 

---

## 2. 입력 받기: input() 함수
출력하는 법을 배웠으니, 이제 컴퓨터가 사용자의 말을 듣게 만들어 봅시다. input() 함수는 사용자가 키보드로 입력한 내용을 컴퓨터가 기억할 수 있게 해줍니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
name = input("Enter your name: ")
print("Hello, ", name)
```
> {: .result }
> > Enter your name: **KHU MSE**  
> > Hello, KHU MSE

### **🤔 Wait and Think!**
{: .text-red-200 }

- `name=`은 무슨 뜻일까요?  
사용자가 입력한 값을 name이라는 이름의 상자(변수)에 담아두겠다는 뜻입니다.
- `input()` 안의 글자들은?  
사용자에게 무엇을 입력해야 할지 알려주는 안내 문구(Prompt)입니다.

{: .highlight }
>💡 **사용자와 소통하는 창구, input()** 
> - `input()` 함수는 사용자가 입력을 마치고 Enter 키를 누를 때까지 기다립니다.
> - 입력받은 내용은 기본적으로 **문자열 (String)**로 취급됩니다. 
> - 만약 숫자를 입력받아 계산하고 싶다면, 나중에 배울 `int()`나 `float()` 같은 함수로 데이터의 유형을 바꿔야 합니다.

---

## 3. 간단한 계산기 
컴퓨터의 가장 본질적인 기능인 '계산'을 시켜봅시다. 숫자 두 개를 입력받아 더하는 프로그램을 만들어 보겠습니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

아래 코드를 실행하고 숫자 10과 20을 입력해 보세요.

```python
num1 = input("Enter the 1st number: ")
num2 = input("Enter the 2nd number: ")
sum = num1 + num2
print("The sum of two numbers: ", sum)
```

### **🤔 Wait and Think!**
{: .text-red-200 }
결과가 예상한대로 나왔나요? 1020이라는 이상한 결과를 얻었을 겁니다.
그 이유는 파이썬이 input()으로 받은 모든 데이터를 일단 **글자(문자열)**로 취급하기 때문입니다. "10"이라는 글자와 "20"이라는 글자를 더하니, 글자끼리 이어 붙여진 것입니다.

### **✅ 올바른 계산기 코드 
진짜 숫자로 계산하려면, 입력받은 글자를 **숫자형**로 변환해 주는 과정이 필요합니다.

```python
# int()를 사용하여 글자를 숫자로 바꿉니다.
num1 = int(input("Enter the 1st number: "))
num2 = int(input("Enter the 2nd number: "))
sum = num1 + num2
print("The sum of two numbers: ", sum)
```

{: .highlight }
>💡 **데이터 유형의 중요성** 
> - 프로그래밍을 할 때는 내가 다루는 데이터가 **글자('10')**인지 **숫자(10)**인지 항상 구분해야 합니다.

### **🤔 Wait and Think!**
{: .text-red-200 }
1.2 와 3.4 의 합도 위의 계산기 코드로 계산이 되나요?
실수들의 합을 계산하려면 어떻게 수정을 해야할까요?