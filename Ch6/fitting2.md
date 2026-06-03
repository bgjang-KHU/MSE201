---
layout: default
title: Curve Fitting
parent: Ch6. 데이터 시각화 및 분석
nav_order: 3
---

# **Curve Fitting 실습 (1)**

지난 시간에는 직접 만든 데이터에 curve fitting을 적용했습니다. 이번 시간에는 **파일로 저장된 실험 데이터를 읽어와서** fitting을 수행하고, 그 결과로 얻은 파라미터가 실제 물리적 의미를 갖는지 확인해봅니다. 이미 알려진 참고값과 비교하면서 fitting이 얼마나 잘 동작하는지 직접 검증해보세요.

---


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
| `plot_results(V, gases_data, popts, names)` | V, 압력 리스트, popt 리스트, 기체명 리스트 | 없음 | 데이터와 피팅 곡선 시각화 |

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

### **3. `plot_results(V, gases_data, popts, names)` — 시각화**

- **입력**: `V` — 부피 배열, `gases_data` — 각 기체의 압력 리스트, `popts` — 각 기체의 `popt` 리스트, `names` — 기체명 리스트
- **반환**: 없음
- `scatter()`로 데이터 점을 그리고, `plot()`으로 피팅 곡선을 겹쳐 그립니다.
- 세 기체를 서로 다른 색으로 표시합니다.

```python
def plot_results(V, gases_data, popts, names):

    함수를 완성하세요.
```

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

plot_results(V,
             [P_N2, P_CO2, P_H2O],
             [popt_N2, popt_CO2, popt_H2O],
             ['N2', 'CO2', 'H2O'])
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

def plot_results(V, gases_data, popts, names):
    colors = ['blue', 'red', 'green']
    plt.figure(figsize=(8, 6))
    for P, popt, name, color in zip(gases_data, popts, names, colors):
        plt.scatter(V, P, c=color, s=10, alpha=0.5, label=f'{name} data')
        V_fine = np.linspace(V.min(), V.max(), 200)
        plt.plot(V_fine, vdw(V_fine, *popt), c=color, linewidth=2, label=f'{name} fit')
    plt.xlabel('V (L/mol)')
    plt.ylabel('P (atm)')
    plt.title('van der Waals Gas: P-V Curve Fitting (T = 290 K)')
    plt.legend(loc=1, fontsize=8)
    plt.grid(True)
    plt.show()

data  = np.loadtxt('vdW_data.txt', comments='#')
V     = data[:, 0]
P_N2  = data[:, 1]
P_CO2 = data[:, 2]
P_H2O = data[:, 3]

popt_N2  = fit_gas(V, P_N2,  'N2',  [1.39, 0.039])
popt_CO2 = fit_gas(V, P_CO2, 'CO2', [3.59, 0.043])
popt_H2O = fit_gas(V, P_H2O, 'H2O', [5.46, 0.031])

plot_results(V,
             [P_N2, P_CO2, P_H2O],
             [popt_N2, popt_CO2, popt_H2O],
             ['N2', 'CO2', 'H2O'])
```
</details>

