---
layout: default
title: Numpy Arrays 소개
parent: Ch2. 변수와 기본 자료구조
nav_order: 9
---
# NumPy: 고성능 과학 계산의 시작

---
**NumPy (Numerical Python)**는 Python 과학 계산의 가장 기본이 되는 모듈입니다. 내부적으로 C 언어로 구현되어 있어 리스트보다 훨씬 빠르고 효율적으로 대량의 데이터를 처리합니다.

## **1. 넘파이 불러오기 및 배열 생성**
<br>
Numpy를 사용하려면 먼저 모듈을 불러와야 합니다. 업계 표준 관례에 따라 `np`라는 별칭을 사용합니다.

### **🚀 TRY IT!**
{: .text-blue-200 }

```python
import numpy as np # 

# 1차원 배열 (Vector) 생성
x = np.array([7, 8, 9]) 
# 2차원 배열 (Matrix) 생성: 중첩 리스트 활용
y = np.array([[1, 2, 3], [4, 5, 6]]) 

print(x)
print(y)
```
> {: .result .fs-3 }
> > [7 8 9]  
> > [[1 2 3]  
> >  [4 5 6]]

## **2. 배열의 속성: 모양(Shape)과 크기(Size)**
<br>
배열이 몇 행 몇 열인지 확인하는 것은 데이터 분석의 첫걸음입니다. 

{: .highlight }
몇 번을 강조해도 지나치지 않습니다. 데이터의 유형, 모양, 크기를 확인하는 것은 데이터 분석의 첫걸음입니다!!!

- `.shape`: 배열의 차원을 튜플 `(행, 열)` 형태로 반환합니다. (속성이므로 괄호를 쓰지 않습니다.)
- `.size`: 배열에 담긴 전체 요소의 개수를 반환합니다.

### **🚀 TRY IT!**
{: .text-blue-200 }
위에서 만들었던 numpy array의 모양과 크기를 확인해봅시다.

```python
print(y.shape) # (2, 3) -> 2행 3열 
print(y.size)  # 6 -> 총 6개의 데이터 
```