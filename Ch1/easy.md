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
name = input("당신의 이름을 입력하세요: ")
print("Hello, ", name)
```
> {: .result }
> > 당신의 이름을 입력하세요: **KHU MSE**  
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
> - 만약 숫자를 입력받아 계산하고 싶다면, 나중에 배울 int()나 float() 같은 함수로 데이터의 유형을 바꿔야 합니다.