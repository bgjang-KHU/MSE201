---
layout: default
title: Curve Fitting
parent: Ch6. 데이터 시각화 및 분석
nav_order: 3
---

# Curve Fitting

---

**Curve Fitting**은 노이즈가 섞인 데이터에서 원래 함수의 파라미터를 추정하는 방법입니다. SciPy의 `curve_fit()`을 사용하면 다양한 함수 꼴에 대해 피팅을 수행할 수 있습니다.

```python
import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt
```

---

## **🔧 `curve_fit()` 기본 사용법**


```python
popt, pcov = curve_fit(func, x, y)
```

| 인자/반환값 | 설명 |
|---|---|
| `func` | 피팅할 함수 — **첫 번째 인자가 독립변수(x)**, 나머지는 피팅할 파라미터 |
| `x` | 독립 변수 데이터 |
| `y` | 종속 변수 데이터 |
| `popt` | 최적 파라미터 배열 |
| `pcov` | 공분산 행렬 — 파라미터의 불확도 정보 |

`curve_fit()`은 첫 번째 인자를 독립변수로 인식합니다. 함수는 반드시 다음 구조로 정의해야 합니다.

```python
def func(x, a, b):      # 첫 번째 인자: 독립변수, 나머지: 피팅할 파라미터 함수에 맞게 사용하면 됩니다. 
    return a * x + b    # 예시 함수 파라미터 2개
```

변수명이 꼭 `x`일 필요는 없지만, **독립변수가 항상 첫 번째 위치**에 와야 합니다.


### **🔑 핵심 포인트: `popt`와 `pcov`**

`popt`는 피팅으로 추정된 파라미터 값들입니다. 함수가 `func(x, a, b)`이면 `popt[0]`이 a, `popt[1]`이 b입니다.

`pcov`는 공분산 행렬로, 대각선 원소의 제곱근이 각 파라미터의 **표준편차(불확도)**입니다.

```python
perr = np.sqrt(np.diag(pcov))   # 각 파라미터의 표준편차
```

예를 들어 `a = 1.03 ± 0.03`처럼 파라미터가 얼마나 신뢰할 수 있는지를 나타냅니다. 데이터 노이즈가 클수록, 데이터 수가 적을수록 불확도가 커집니다.

---

## **📈 선형 피팅**

가장 간단한 예시로 $f(x) = ax + b$ 형태의 데이터를 피팅합니다. 먼저 원래 함수로 데이터를 만들고, 노이즈를 추가한 뒤 피팅으로 원래 파라미터를 복원합니다.

```python
def func(x, a, b):
    return a * x + b

# 원래 함수: a=1, b=2
x  = np.linspace(0, 10, 100)
y  = func(x, 1, 2)

# 노이즈 추가
np.random.seed(1)
yn = y + 0.9 * np.random.normal(size=len(x))

# 피팅
popt, pcov = curve_fit(func, x, yn)
perr = np.sqrt(np.diag(pcov))

print(f'a = {popt[0]:.4f} ± {perr[0]:.4f}')
print(f'b = {popt[1]:.4f} ± {perr[1]:.4f}')
```

```
a = 1.0298 ± 0.0274
b = 1.9056 ± 0.1588
```

피팅 결과를 원래 데이터와 함께 시각화합니다.

```python
plt.figure(figsize=(8, 8))
plt.scatter(x, yn, marker='o', label='Data')
plt.plot(x, y,                        color='blue', linewidth=2, label='Original')
plt.plot(x, func(x, popt[0], popt[1]), color='red',  linewidth=2, label='Fitting')
plt.legend(loc=0)
plt.xlabel('x')
plt.ylabel('y')
plt.show()
```

### **🚀 TRY IT!**

노이즈 크기를 `0.9`에서 `3.0`으로 바꿔보세요. `popt`와 `perr` 값이 어떻게 달라지나요?

---

## **🔔 가우시안 피팅**

실험 데이터에서 자주 나타나는 피크(peak) 형태는 **가우시안 함수**로 피팅할 수 있습니다.

$$f(x) = a \exp\left(-\frac{(x-b)^2}{2c^2}\right)$$

- $a$: 피크 높이 (height)
- $b$: 피크 중심 위치 (center)
- $c$: 표준편차, 피크의 너비를 결정 (width)

```python
def func(x, a, b, c):
    return a * np.exp(-((x - b)**2) / (2 * c**2))

# 원래 함수: a=1, b=5, c=2
x  = np.linspace(0, 10, 100)
y  = func(x, 1, 5, 2)

np.random.seed(1)
yn = y + 0.1 * np.random.normal(size=len(x))

popt, pcov = curve_fit(func, x, yn)
print(popt)   # [a, b, c] 순서로 출력
```

피팅 결과 시각화:

```python
plt.figure(figsize=(8, 8))
plt.scatter(x, yn, marker='o', label='Data')
plt.plot(x, y,             color='blue', linewidth=2, label='Original')
plt.plot(x, func(x, *popt), color='red',  linewidth=2, label='Fitting')
plt.legend(loc=0)
plt.xlabel('x')
plt.ylabel('y')
plt.show()
```

{: .highlight }
> 💡 **TIP — `func(x, *popt)`**
>
> `*popt`는 배열을 개별 인자로 풀어서 전달합니다. `popt = [a, b, c]`일 때 `func(x, *popt)`는 `func(x, a, b, c)`와 같습니다. 파라미터가 많을 때 편리합니다.

---

## **🎯 initial guess — 초기값 지정**

복잡한 함수(예: 피크가 여러 개)는 `curve_fit()`이 잘못된 해에 수렴할 수 있습니다. 이럴 때 `p0`로 **초기 추정값**을 지정하면 올바른 해를 찾는 데 도움이 됩니다.

아래는 두 개의 가우시안이 합쳐진 함수의 예시입니다.

```python
def func(x, a0, b0, c0, a1, b1, c1):
    return (a0 * np.exp(-((x - b0)**2) / (2 * c0**2)) +
            a1 * np.exp(-((x - b1)**2) / (2 * c1**2)))

x  = np.linspace(0, 20, 200)
y  = func(x, 1, 3, 1, -2, 15, 0.5)
yn = y + 0.1 * np.random.normal(size=len(x))

# 초기값 지정
initial_guess = [1, 3, 1, 1, 15, 1]
popt, pcov = curve_fit(func, x, yn, p0=initial_guess)

plt.figure(figsize=(8, 8))
plt.scatter(x, yn, marker='o', label='Data')
plt.plot(x, y,             color='blue', linewidth=2, label='Original')
plt.plot(x, func(x, *popt), color='red',  linewidth=2, label='Fitting')
plt.legend(loc=0)
plt.show()
```

### **🤔 Wait and Think!**

`p0=initial_guess`를 제거하고 실행해보세요. 피팅 결과가 달라지나요?

---

## **⛓️ bounds — 파라미터 범위 제한**

파라미터가 특정 범위 안에 있어야 할 때 `bounds`로 제한을 걸 수 있습니다. 잘못된 `bounds`는 오히려 피팅을 망칩니다.

```python
def func(x, a, b):
    return a * (x**3) + b

x  = np.arange(-10, 10, 0.1)
y  = func(x, 3, -20)
yn = y + 50*x * np.random.normal(size=len(x))

# 올바른 bounds: a는 0~5, b는 -50~0
popt,  pcov  = curve_fit(func, x, yn, bounds=([0, -50], [5,  0]))

# 잘못된 bounds: a를 5~10으로 제한
popt2, pcov2 = curve_fit(func, x, yn, bounds=([5,  00], [10, 50]))

plt.figure(figsize=(8, 8))
plt.scatter(x, yn, marker='o', label='Data')
plt.plot(x, func(x, *popt),  color='blue', linewidth=2, label='Fitting')
plt.plot(x, func(x, *popt2), color='red',  linewidth=2, label='Wrong bounds')
plt.legend(loc=0)
plt.show()
```

{: .highlight }
> 💡 **TIP — `bounds` 형식**
>
> `bounds=([하한값들], [상한값들])` 형식으로 지정합니다. 파라미터가 `a, b` 두 개라면 `bounds=([a_min, b_min], [a_max, b_max])`입니다.

### **🔑 핵심 정리**

| 기능 | 코드 |
|---|---|
| 기본 피팅 | `popt, pcov = curve_fit(func, x, y)` |
| 파라미터 불확도 | `perr = np.sqrt(np.diag(pcov))` |
| 초기값 지정 | `curve_fit(func, x, y, p0=initial_guess)` |
| 범위 제한 | `curve_fit(func, x, y, bounds=([하한], [상한]))` |
| 피팅값으로 함수 계산 | `func(x, *popt)` |