---
layout: default
title: 2D Contour Plot
parent: Ch6. 데이터 시각화 및 분석
nav_order: 2
---

# 2D Contour Plot


지난 시간에는 1D 데이터를 선 그래프로 그렸습니다. 이번에는 **2D 함수**, 즉 $z = f(x, y)$ 형태의 함수를 평면 위에 시각화하는 **등고선 그래프(Contour Plot)**를 배웁니다.

---

## **🗺️ meshgrid 이해하기**

1D 그래프는 x 배열 하나로 충분했지만, 2D 함수는 x와 y 모든 조합에 대해 z 값을 계산해야 합니다. `np.meshgrid()`는 이 격자(grid)를 만들어주는 함수입니다.

```python
x = np.linspace(0, 2, 3)   # [0, 1, 2]
y = np.linspace(0, 2, 3)   # [0, 1, 2]

X, Y = np.meshgrid(x, y)
```

결과를 출력해보면 X와 Y가 어떻게 생겼는지 알 수 있습니다.

```python
print("X:\n", X)
print("Y:\n", Y)
```

```
X:
 [[0. 1. 2.]
  [0. 1. 2.]
  [0. 1. 2.]]

Y:
 [[0. 0. 0.]
  [1. 1. 1.]
  [2. 2. 2.]]
```

X는 x 값이 행 방향으로, Y는 y 값이 열 방향으로 반복됩니다. 이렇게 하면 격자의 모든 점 `(X[i][j], Y[i][j])`에서 함수값을 한 번에 계산할 수 있습니다.

```python
Z = X**2 + Y**2

print("Z:\n", Z)
```

```
Z:
 [[0. 1. 4.]
  [1. 2. 5.]
  [4. 5. 8.]]
```

예를 들어 점 `(1, 2)`에서 $z = 1^2 + 2^2 = 5$ — 배열에서 `Z[2][1] = 5`로 확인됩니다.

### **🤔 Wait and Think!**

`X`와 `Y`의 shape은 무엇일까요? 아래 코드로 확인해보세요.

```python
x = np.linspace(-1.5, 1.5, 400)
y = np.linspace(-1.5, 1.5, 400)
X, Y = np.meshgrid(x, y)

print(X.shape)
print(Y.shape)
```

---

## **📈 `plt.contour()` — 등고선 그리기**

`plt.contour(X, Y, Z)`는 Z 값이 같은 점들을 연결한 등고선을 그립니다. `levels`를 지정하지 않으면 **matplotlib이 자동으로 여러 등고선**을 선택합니다.

```python
x = np.linspace(-1.5, 1.5, 400)
y = np.linspace(-1.5, 1.5, 400)
X, Y = np.meshgrid(x, y)

Z = X**2 + Y**2

plt.figure(figsize=(6, 6))
plt.contour(X, Y, Z)          # levels 없이 → 자동으로 여러 등고선
plt.xlabel('x')
plt.ylabel('y')
plt.title('contour without levels')
plt.grid(True)
plt.show()
```

$z = x^2 + y^2$는 원점을 중심으로 한 동심원이므로, 여러 개의 원형 등고선이 그려집니다.

### **🔑 핵심 포인트: `levels`로 원하는 등고선만**

`levels=[값]` 을 지정하면 **그 값에 해당하는 등고선만** 그릴 수 있습니다.

$x^2 + y^2 = 1$ 인 원 하나만 그리려면 `levels=[1]`을 넣으면 됩니다.

```python
plt.figure(figsize=(6, 6))
plt.contour(X, Y, Z, levels=[1], colors='black', linewidths=2)
plt.xlabel('x')
plt.ylabel('y')
plt.title('$x^2 + y^2 = 1$')
plt.grid(True)
plt.show()
```

여러 개의 등고선을 원하면 리스트에 값을 추가하면 됩니다.

```python
plt.figure(figsize=(6, 6))
plt.contour(X, Y, Z, levels=[0.25, 0.5, 1.0, 1.5])
plt.xlabel('x')
plt.ylabel('y')
plt.title('여러 등고선')
plt.grid(True)
plt.show()
```

### **🚀 TRY IT!**

$z = x^2 + y^2$ 대신 $z = x^2 - y^2$ (안장면, saddle surface)를 정의하고 등고선을 그려보세요. `levels=0` 인 선이 어떻게 생겼는지 확인해보세요.

---

## **🎨 `plt.contourf()` — 색상으로 채우기**

`plt.contourf()`는 등고선 사이를 **색상으로 채워** 함수값의 분포를 한눈에 볼 수 있습니다. `plt.colorbar()`를 추가하면 색상과 값의 대응 관계를 표시합니다.

```python
plt.figure(figsize=(6, 6))
cs = plt.contourf(X, Y, Z, levels=20)   # levels=20: 20개 구간으로 나눠 채우기
plt.colorbar(cs)                         # 색상 범례 추가
plt.xlabel('x')
plt.ylabel('y')
plt.title('contourf — $x^2 + y^2$')
plt.show()
```

`contour()`와 `contourf()`를 함께 쓰면 채운 색 위에 등고선을 겹쳐 그릴 수 있습니다.

```python
plt.figure(figsize=(6, 6))
cs = plt.contourf(X, Y, Z, levels=20)
plt.contour(X, Y, Z, levels=[1], colors='white', linewidths=2)
plt.colorbar(cs)
plt.xlabel('x')
plt.ylabel('y')
plt.title('contourf + contour')
plt.show()
```

### **🚀 TRY IT!**

$z = \sin(x) \cdot \cos(y)$ 함수를 `contourf()`로 그리고 `colorbar()`를 추가해보세요.


---

## **🔑 핵심 정리**

| 함수 | 설명 |
|---|---|
| `np.meshgrid(x, y)` | x, y 1D 배열로 2D 격자 생성 |
| `plt.contour(X, Y, Z)` | 등고선 그리기 |
| `plt.contour(..., levels=[값])` | 특정 값의 등고선만 그리기 |
| `plt.contourf(X, Y, Z)` | 색상으로 채운 등고선 그리기 |
| `plt.colorbar(cs)` | 색상 범례 추가 |

---


## **🎲 Monte Carlo 시뮬레이션 — 원의 넓이 구하기**

### **아이디어**

반지름 1인 원이 한 변의 길이가 2인 정사각형 안에 딱 맞게 들어있다고 생각해봅시다. 이 정사각형 안에 점을 **완전히 랜덤하게** 뿌리면, 원 안에 떨어지는 점의 비율은 넓이의 비율과 같을 것입니다.

$$\frac{\text{원 안의 점 수}}{\text{전체 점 수}} \approx \frac{\text{원의 넓이}}{\text{정사각형의 넓이}} = \frac{\pi r^2}{(2r)^2} = \frac{\pi}{4}$$

따라서 점을 많이 뿌릴수록 $\pi$를 점점 정확하게 추정할 수 있습니다.

$$\pi \approx 4 \times \frac{\text{원 안의 점 수}}{\text{전체 점 수}}$$

---

### **① 점 하나 생성하고 판별하기**

`random.uniform(a, b)`는 a와 b 사이의 임의의 실수를 반환합니다. x, y 좌표를 각각 $-1 \sim 1$ 사이에서 뽑고, 원 방정식 $x^2 + y^2 \leq 1$로 안/밖을 판별합니다.

```python
import random

x = random.uniform(-1, 1)
y = random.uniform(-1, 1)

if x**2 + y**2 <= 1:
    print(f'({x:.3f}, {y:.3f}) → 원 안')
else:
    print(f'({x:.3f}, {y:.3f}) → 원 밖')
```

### **🚀 TRY IT!**

위 코드를 여러 번 실행해보세요. 실행할 때마다 다른 결과가 나오나요?

---

### **② n개의 점으로 확장하기**

`for`문으로 n번 반복하면서, 원 안에 들어온 점과 밖의 점을 각각 리스트에 담습니다.

```python
import random

n = 1000
inx, iny   = [], []   # 원 안의 점
outx, outy = [], []   # 원 밖의 점

for i in range(n):
    x = random.uniform(-1, 1)
    y = random.uniform(-1, 1)

    if x**2 + y**2 <= 1:
        inx.append(x)
        iny.append(y)
    else:
        outx.append(x)
        outy.append(y)

print(f'전체: {n}개  |  원 안: {len(inx)}개  |  원 밖: {len(outx)}개')
```

---

### **③ 원의 넓이 계산하기**

원 안의 점 수를 전체 점 수로 나누고 4를 곱하면 원의 넓이(≈ π)를 추정할 수 있습니다.

```python
import math

area = len(inx) / n * 4
error = (math.pi - area) / math.pi * 100

print(f'추정 넓이: {area:.4f}')
print(f'실제 π:   {math.pi:.4f}')
print(f'오차율:   {error:.4f}%')
```

### **🤔 Wait and Think!**

n을 10, 100, 1000, 10000으로 바꿔가며 실행해보세요. n이 커질수록 추정값이 어떻게 변하나요?

---

### **④ 시각화하기**

앞에서 배운 `contour()`로 원을 그리고, `plt.scatter()`로 점들을 색깔별로 표시합니다.

```python
import math
import numpy as np
import random
import matplotlib.pyplot as plt

n = 1000
inx, iny   = [], []
outx, outy = [], []

for i in range(n):
    x = random.uniform(-1, 1)
    y = random.uniform(-1, 1)
    if x**2 + y**2 <= 1:
        inx.append(x)
        iny.append(y)
    else:
        outx.append(x)
        outy.append(y)

# 원 그리기 (contour)
x = np.linspace(-1, 1, 400)
y = np.linspace(-1, 1, 400)
X, Y = np.meshgrid(x, y)
Z = X**2 + Y**2

plt.figure(figsize=(6, 6))
plt.contour(X, Y, Z, levels=[1], colors='black', linewidths=2)

# 점 그리기 (scatter)
plt.scatter(outx, outy, c='b', s=2, label='Outside')
plt.scatter(inx,  iny,  c='r', s=2, label='Inside')

plt.xlabel('x')
plt.ylabel('y')
plt.legend()
plt.grid(True)
plt.show()

area = len(inx) / n * 4
error = (math.pi - area) / math.pi * 100

print(f'n = {n} | 추정 넓이 = {area:.4f} | 오차율 = {error:.4f}%')
```

> 💡 **TIP — `plt.scatter()`**
>
> `plt.scatter(x, y)`는 점들을 흩뿌려 표시합니다. `c`로 색상, `s`로 점 크기를 지정할 수 있습니다. 점이 많을수록 `s`를 작게 설정하는 것이 좋습니다.

### **🚀 TRY IT!**

n을 10000으로 바꿔보세요. 시각화가 어떻게 달라지나요? 추정값은 더 정확해지나요?


---

## **✍️ 연습 문제 — Monte Carlo 시뮬레이션 함수로 구현하기**

본문에서 단계별로 작성했던 몬테카를로 원 넓이 구하기를 **4개의 함수**로 나누어 구현합니다.

```python
import numpy as np
import math
import random
import matplotlib.pyplot as plt
```

---

### **1. `calc_error(estimated_area)` — 오차율 계산**

- **입력**: `estimated_area` — 추정된 원의 넓이 (float)
- **반환**: 오차율 (float, 단위 %)
- `math.pi`를 기준값으로 오차율을 계산하여 반환합니다.

$$\text{오차율} = \frac{\pi - \text{추정값}}{\pi} \times 100$$

```python
def calc_error(estimated_area, reference=math.pi):

    함수를 완성하세요.
```

<!--
<details markdown="1">
<summary>예시 풀이</summary>

```python
def calc_error(estimated_area, reference=math.pi):
    return (reference - estimated_area) / reference * 100
```
</details>
-->

---

### **2. `MC(n)` — 몬테카를로 시뮬레이션**

- **입력**: `n` — 생성할 점의 개수 (int)
- **반환**: `inx, iny, outx, outy` — 원 안/밖의 x, y 좌표 리스트 4개
- `random.uniform(-1, 1)`로 x, y 좌표를 생성하고, $x^2 + y^2 \leq 1$ 조건으로 원 안/밖을 판별합니다.
- 추정 넓이와 오차율을 출력합니다. (`calc_error()` 활용)

**출력 예시**
```
n = 1000 | Estimated area = 3.180000 | Error = -1.2225%
```

```python
def MC(n):
    inx, iny, outx, outy = [], [], [], []

    함수를 완성하세요.

    return inx, iny, outx, outy
```

<!--
<details markdown="1">
<summary>예시 풀이</summary>

```python
def MC(n):
    inx, iny, outx, outy = [], [], [], []

    for i in range(n):
        x = random.uniform(-1, 1)
        y = random.uniform(-1, 1)
        if x**2 + y**2 <= 1:
            inx.append(x)
            iny.append(y)
        else:
            outx.append(x)
            outy.append(y)

    area = len(inx) / n * 4
    error = calc_error(area)
    print(f'n = {n} | Estimated area = {area:.6f} | Error = {error:.4f}%')

    return inx, iny, outx, outy
```
</details>
-->

---

### **3. `figure(inx, iny, outx, outy)` — 시각화**

- **입력**: `inx, iny, outx, outy` — `MC()`에서 반환된 4개의 리스트
- **반환**: 없음
- `meshgrid`와 `contour()`로 단위원을 그리고, `scatter()`로 원 안/밖의 점을 서로 다른 색으로 표시합니다.
  - 원 안의 점: 빨간색 (`c='r'`)
  - 원 밖의 점: 파란색 (`c='b'`)

```python
def figure(inx, iny, outx, outy):

    함수를 완성하세요.
```

<!--
<details markdown="1">
<summary>예시 풀이</summary>

```python
def figure(inx, iny, outx, outy):
    x = np.linspace(-1, 1, 400)
    y = np.linspace(-1, 1, 400)
    X, Y = np.meshgrid(x, y)
    Z = X**2 + Y**2

    plt.figure(figsize=(8, 8))
    plt.contour(X, Y, Z, levels=[1], colors='black', linewidths=2)
    plt.scatter(outx, outy, c='b', s=2, label='Outside')
    plt.scatter(inx,  iny,  c='r', s=2, label='Inside')
    plt.xlabel('x', fontsize=12)
    plt.ylabel('y', fontsize=12)
    plt.grid(True)
    plt.legend()
    plt.show()
```
</details>
-->

---

### **4. `sampling(sample_list)` — 여러 n에 대한 결과 파일 저장**

- **입력**: `sample_list` — 시뮬레이션할 n 값들의 리스트
- **반환**: 없음
- 리스트의 각 n에 대해 `MC()`를 호출하고, 추정 넓이와 오차율을 `sampling.txt`에 저장합니다.

**저장 파일 형식 (`sampling.txt`)**

```
Samples	Estimated Area	Error (%)
      10	         3.2000	   -1.8592
     100	         2.9600	    5.7803
    1000	         3.1640	   -0.7132
   10000	         3.1296	    0.3817
   20000	         3.1396	    0.0634
   30000	         3.1423	   -0.0215
   40000	         3.1414	    0.0061
   50000	         3.1394	    0.0711
```

```python
def sampling(sample_list):

    함수를 완성하세요.
```

<!--
<details markdown="1">
<summary>예시 풀이</summary>

```python
def sampling(sample_list):
    f = open("sampling.txt", "w")
    f.write("Samples\tEstimated Area\tError (%)\n")

    for n in sample_list:
        inx, iny, outx, outy = MC(n)
        area = len(inx) / n * 4
        error = calc_error(area)
        f.write(f"{n:8d}\t{area:15.4f}\t{error:10.4f}\n")

    f.close()
```
</details>
-->

---

### **🚀 전체 실행**

```python
n = 10000
inx, iny, outx, outy = MC(n)
figure(inx, iny, outx, outy)

list1 = [10, 100, 1000, 10000, 20000, 30000, 40000, 50000]
sampling(list1)
```

> 🤔 **생각해보기**: `sampling.txt`를 열어보면 n이 커질수록 오차율이 어떻게 변하나요? n=50000일 때도 오차가 완전히 0이 되지 않는 이유는 무엇일까요?