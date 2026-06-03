---
layout: default
title: Fitting 실습 (1)
parent: Ch6. 데이터 시각화 및 분석
nav_order: 4
---

# **Curve Fitting 실습 (1)**

지난 시간에는 직접 만든 데이터에 curve fitting을 적용했습니다. 이번 시간에는 **파일로 저장된 실험 데이터를 읽어와서** fitting을 수행하고, 그 결과로 얻은 파라미터가 실제 물리적 의미를 갖는지 확인해봅니다. 이미 알려진 참고값과 비교하면서 fitting이 얼마나 잘 동작하는지 직접 검증해보세요.

---

---

## **연습 문제 2 — 포물선 운동 데이터 피팅 (`projectile.txt`)**

초기 속도 $v_0$, 발사각 $\theta$로 던진 물체의 운동 데이터입니다. 공기 저항을 무시하면 x, y 방향 운동은 다음과 같습니다.

$$x(t) = v_0 \cos\theta \cdot t$$

$$y(t) = v_0 \sin\theta \cdot t - \frac{1}{2}g t^2$$

t-y 데이터에 fitting을 적용하면 **초기 수직 속도** $v_0\sin\theta$와 **중력가속도** $g$를 추정할 수 있습니다.

- [projectile.txt 다운로드](https://bgjang-khu.github.io/MSE201/Ch6/data/projectile.txt)

파일은 다음 3개의 컬럼으로 구성되어 있습니다.

```
# Projectile motion data
# v0 = 20 m/s, theta = 45 deg
# t(s)    x(m)    y(m)
0.00    0.0497    -0.0292
0.10    1.4004    1.3050
...
```

피팅에 사용할 모델 함수는 다음과 같습니다.

```python
def projectile(t, v0_sin, g):
    return v0_sin * t - 0.5 * g * t**2
```

- `v0_sin`: $v_0\sin\theta$ — 초기 수직 속도 (m/s)
- `g`: 중력가속도 (m/s²)

피팅 결과를 실제 중력가속도 $g = 9.8$ m/s²와 비교해보세요.

---

### **함수 설계**

| 함수 | 입력 | 반환 | 역할 |
|---|---|---|---|
| `plot_trajectory(x, y)` | x, y 배열 | 없음 | x-y 궤적 시각화 |
| `fit_projectile(t, y)` | t, y 배열 | `popt` | t-y 피팅, $v_0\sin\theta$와 $g$ 추정 |
| `plot_fitting(t, y, popt)` | t, y 배열, popt | 없음 | t-y 데이터 + 피팅 곡선 시각화 |

---

### **1. `plot_trajectory(x, y)` — 포물선 궤적 시각화**

- **입력**: `x` — 수평 거리 배열, `y` — 높이 배열
- **반환**: 없음
- `scatter()`로 x-y 궤적을 그립니다. 피팅과는 무관하게 실제 물체의 궤적을 확인합니다.
- x축: 수평 거리 (m), y축: 높이 (m)

```python
import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt

def projectile(t, v0_sin, g):
    return v0_sin * t - 0.5 * g * t**2

def plot_trajectory(x, y):

    함수를 완성하세요.
```

---

### **2. `fit_projectile(t, y)` — 피팅 수행**

- **입력**: `t` — 시간 배열, `y` — 높이 배열
- **반환**: `popt` — 피팅된 파라미터 배열
- `curve_fit()`으로 $v_0\sin\theta$와 $g$를 추정하고 결과를 출력합니다.
- 초기값 `p0=[10, 9.0]`을 사용합니다.
- `perr = np.sqrt(np.diag(pcov))`로 불확도를 함께 출력합니다.

**출력 예시**

```
v0sinθ = 14.0847 ± 0.0420 m/s
g      =  9.7534 ± 0.0380 m/s²
```

```python
def fit_projectile(t, y):

    함수를 완성하세요.

    return popt
```

---

### **3. `plot_fitting(t, y, popt)` — t-y 피팅 결과 시각화**

- **입력**: `t` — 시간 배열, `y` — 높이 배열, `popt` — 피팅 파라미터
- **반환**: 없음
- `scatter()`로 데이터 점을, `plot()`으로 피팅 곡선을 겹쳐 그립니다.
- x축: 시간 (s), y축: 높이 (m)

```python
def plot_fitting(t, y, popt):

    함수를 완성하세요.
```

---

### **🚀 전체 실행**

```python
data = np.loadtxt('projectile.txt', comments='#')
t = data[:, 0]
x = data[:, 1]
y = data[:, 2]

plot_trajectory(x, y)

popt = fit_projectile(t, y)

plot_fitting(t, y, popt)
```



<details markdown="1">
<summary>예시 풀이</summary>

```python
import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt

def projectile(t, v0_sin, g):
    return v0_sin * t - 0.5 * g * t**2

def plot_trajectory(x, y):
    plt.figure(figsize=(8, 5))
    plt.scatter(x, y, c='blue', s=20, label='Data')
    plt.xlabel('x (m)')
    plt.ylabel('y (m)')
    plt.title('Projectile Trajectory')
    plt.legend()
    plt.grid(True)
    plt.show()

def fit_projectile(t, y):
    popt, pcov = curve_fit(projectile, t, y, p0=[10, 9.0])
    perr = np.sqrt(np.diag(pcov))
    print(f'v0sinθ = {popt[0]:.4f} ± {perr[0]:.4f} m/s')
    print(f'g      = {popt[1]:.4f} ± {perr[1]:.4f} m/s²')
    return popt

def plot_fitting(t, y, popt):
    t_fine = np.linspace(t.min(), t.max(), 200)
    plt.figure(figsize=(8, 5))
    plt.scatter(t, y, c='blue', s=20, label='Data')
    plt.plot(t_fine, projectile(t_fine, *popt), c='red', linewidth=2, label='Fitting')
    plt.xlabel('t (s)')
    plt.ylabel('y (m)')
    plt.title('Projectile Motion: t-y Fitting')
    plt.legend()
    plt.grid(True)
    plt.show()

data = np.loadtxt('projectile.txt', comments='#')
t = data[:, 0]
x = data[:, 1]
y = data[:, 2]

plot_trajectory(x, y)

popt = fit_projectile(t, y)

plot_fitting(t, y, popt)
```
</details>

--


## **✍️ 연습 문제1 — van der Waals 기체 상수 피팅**

실제 기체는 이상기체 방정식 $PV = nRT$를 따르지 않습니다. **van der Waals 방정식**은 분자 간 인력과 분자 자체의 부피를 보정하여 실제 기체의 거동을 더 정확하게 설명합니다.

$$P = \frac{nRT}{V - nb} - a\left(\frac{n}{V}\right)^2$$

- $a$: 분자 간 인력 보정 상수 (압력 보정, 단위: L²·atm/mol²)
- $b$: 분자 자체 부피 보정 상수 (부피 보정, 단위: L/mol)
- $n = 1$ mol, $T = 290$ K, $R = 0.08206$ L·atm/mol·K

- [vdW_data.txt 다운로드](https://bgjang-khu.github.io/MSE201/Ch6/data/vdW_data.txt)

파일은 다음 4개의 컬럼으로 구성되어 있습니다.

```
# van der Waals gas data
# T = 290 K, n = 1 mol, R = 0.08206 L·atm/mol·K
# V(L/mol)    P_N2(atm)    P_CO2(atm)    P_H2O(atm)
0.2000    113.9949    61.4091    3.9355
...
```

### **실제 van der Waals 상수 (참고값)**

| 기체 | $a$ (L²·atm/mol²) | $b$ (L/mol) | 특징 |
|---|---|---|---|
| N₂ | 1.39 | 0.0391 | 이상기체에 가까움 |
| CO₂ | 3.59 | 0.0427 | 중간 정도의 보정 |
| H₂O | 5.46 | 0.0305 | 수소결합으로 인력이 강함 |

피팅 결과를 위 참고값과 비교해보세요.

---

### **함수 설계**

다음 3개의 함수로 나누어 구현합니다.

| 함수 | 입력 | 반환 | 역할 |
|---|---|---|---|
| `vdw(V, a, b)` | V 배열, 파라미터 a, b | P 배열 | van der Waals 방정식 |
| `fit_gas(V, P, name, p0)` | V, P 배열, 기체명, 초기값 | `popt` | 피팅 수행 및 결과 출력 |
| `plot_one(V, P, popt, name, color)` | V, P 배열, popt, 기체명, 색상 | 없음 | 데이터와 피팅 곡선 시각화 |

---

### **1. `vdw(V, a, b)` — van der Waals 방정식**

- **입력**: `V` — 부피 배열, `a`, `b` — 피팅할 파라미터
- **반환**: 압력 배열 `P`
- `curve_fit()`에 전달할 함수입니다. 첫 번째 인자가 독립변수(`V`)임에 주의하세요.

```python
import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt

R = 0.08206   # L·atm/mol·K
T = 290       # K
n = 1         # mol

def vdw(V, a, b):

    함수를 완성하세요.
```

---

### **2. `fit_gas(V, P, name, p0)` — 피팅 수행**

- **입력**: `V` — 부피 배열, `P` — 압력 배열, `name` — 기체명 (문자열), `p0` — 초기값 리스트
- **반환**: `popt` — 피팅된 파라미터 배열
- `curve_fit()`으로 a, b를 추정하고 결과를 출력합니다.
- `bounds=([0, 0.001], [10, 0.2])`로 파라미터 범위를 제한합니다.
- `perr = np.sqrt(np.diag(pcov))`로 불확도를 계산하여 함께 출력합니다.

**출력 예시**

```
N2:  a = 1.6948 ± 0.0819,  b = 0.0489 ± 0.0022
CO2: a = 3.7000 ± 0.0950,  b = 0.0454 ± 0.0026
H2O: a = 5.5863 ± 0.0993,  b = 0.0342 ± 0.0031
```

```python
def fit_gas(V, P, name, p0):

    함수를 완성하세요.

    return popt
```

---

### **3. `plot_one(V, P, popt, name, color)` — 시각화**

- **입력**: `V` — 부피 배열, `P` — 압력 배열, `popt` — 피팅 파라미터, `name` — 기체명, `color` — 색상
- **반환**: 없음
- `scatter()`로 데이터 점을, `plot()`으로 피팅 곡선을 겹쳐 그립니다.
- 기체 하나에 대한 그래프를 그리는 함수입니다. 세 번 호출하여 한 그래프에 겹쳐 그립니다.

````python
def plot_one(V, P, popt, name, color):

    함수를 완성하세요.
````

---

### **🚀 전체 실행**

```python
data  = np.loadtxt('vdW_data.txt', comments='#')
V     = data[:, 0]
P_N2  = data[:, 1]
P_CO2 = data[:, 2]
P_H2O = data[:, 3]

popt_N2  = fit_gas(V, P_N2,  'N2',  [1.39, 0.039])
popt_CO2 = fit_gas(V, P_CO2, 'CO2', [3.59, 0.043])
popt_H2O = fit_gas(V, P_H2O, 'H2O', [5.46, 0.031])

plt.figure(figsize=(8, 6))
plot_one(V, P_N2,  popt_N2,  'N2',  'blue')
plot_one(V, P_CO2, popt_CO2, 'CO2', 'red')
plot_one(V, P_H2O, popt_H2O, 'H2O', 'green')
plt.xlabel('V (L/mol)')
plt.ylabel('P (atm)')
plt.title('van der Waals Gas: P-V Curve Fitting (T = 290 K)')
plt.legend()
plt.grid(True)
plt.show()
```

> 🤔 **생각해보기**: 피팅으로 얻은 a, b 값을 참고값과 비교해보세요. N₂의 a 값이 왜 가장 작을까요? H₂O의 a 값이 큰 이유는 무엇일까요?


<details markdown="1">
<summary>예시 풀이</summary>

```python
import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt

R = 0.08206
T = 290
n = 1

def vdw(V, a, b):
    return n * R * T / (V - n * b) - a * (n / V)**2

def fit_gas(V, P, name, p0):
    popt, pcov = curve_fit(vdw, V, P, p0=p0, bounds=([0, 0.001], [10, 0.2]))
    perr = np.sqrt(np.diag(pcov))
    print(f'{name}: a={popt[0]:.4f}±{perr[0]:.4f}, b={popt[1]:.4f}±{perr[1]:.4f}')
    return popt

def plot_one(V, P, popt, name, color):
    plt.scatter(V, P, c=color, s=10, label=f'{name} data')
    V_fine = np.linspace(V.min(), V.max(), 200)
    plt.plot(V_fine, vdw(V_fine, *popt), c=color, linewidth=2, label=f'{name} fit')

data  = np.loadtxt('vdW_data.txt', comments='#')
V     = data[:, 0]
P_N2  = data[:, 1]
P_CO2 = data[:, 2]
P_H2O = data[:, 3]

popt_N2  = fit_gas(V, P_N2,  'N2',  [1.39, 0.039])
popt_CO2 = fit_gas(V, P_CO2, 'CO2', [3.59, 0.043])
popt_H2O = fit_gas(V, P_H2O, 'H2O', [5.46, 0.031])

plt.figure(figsize=(8, 6))
plot_one(V, P_N2,  popt_N2,  'N2',  'blue')
plot_one(V, P_CO2, popt_CO2, 'CO2', 'red')
plot_one(V, P_H2O, popt_H2O, 'H2O', 'green')
plt.xlabel('V (L/mol)')
plt.ylabel('P (atm)')
plt.title('van der Waals Gas: P-V Curve Fitting (T = 290 K)')
plt.legend()
plt.grid(True)
plt.show()
```
</details>

