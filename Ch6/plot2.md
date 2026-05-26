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

```python
x = np.linspace(-2, 2, 400)
y = np.linspace(-2, 2, 400)
X, Y = np.meshgrid(x, y)

Z =                          # x^2 - y^2

plt.figure(figsize=(6, 6))
plt.contour(X, Y, Z, levels=[0], colors='black', linewidths=2)
plt.xlabel('x')
plt.ylabel('y')
plt.title('$x^2 - y^2 = 0$')
plt.grid(True)
plt.show()
```

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

```python
x = np.linspace(-np.pi, np.pi, 300)
y = np.linspace(-np.pi, np.pi, 300)
X, Y = np.meshgrid(x, y)

Z =                                     # sin(x) * cos(y)

plt.figure(figsize=(6, 6))
cs = plt.contourf(X, Y, Z,       )     # levels 적당히 설정
plt.colorbar(cs)
plt.xlabel('x')
plt.ylabel('y')
plt.title('$\sin(x)\cos(y)$')
plt.show()
```

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

## **✍️ 연습 문제**

### **타원 방정식 그리기**

다음 타원 방정식의 등고선을 그려보세요.

$$\frac{x^2}{4} + \frac{y^2}{9} = 1$$

- `levels=[1]`을 사용하여 타원 하나만 그립니다.
- x 범위: $-3 \sim 3$, y 범위: $-4 \sim 4$
- 제목, 축 이름, 격자를 추가합니다.

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(-3, 3, 400)
y = np.linspace(-4, 4, 400)
X, Y = np.meshgrid(x, y)

Z =                          # x^2/4 + y^2/9

plt.figure(figsize=(5, 7))
plt.contour(X, Y, Z,        )
plt.xlabel(      )
plt.ylabel(      )
plt.title(       )
plt.grid(True)
plt.show()
```

<!--
<details markdown="1">
<summary>예시 풀이</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(-3, 3, 400)
y = np.linspace(-4, 4, 400)
X, Y = np.meshgrid(x, y)

Z = X**2 / 4 + Y**2 / 9

plt.figure(figsize=(5, 7))
plt.contour(X, Y, Z, levels=[1], colors='black', linewidths=2)
plt.xlabel('x')
plt.ylabel('y')
plt.title('$\\frac{x^2}{4} + \\frac{y^2}{9} = 1$')
plt.grid(True)
plt.show()
```
</details>
-->