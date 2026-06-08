---
layout: default
title: Fitting 실습 (2)
parent: Ch6. 데이터 시각화 및 분석
nav_order: 5
---

# **Curve Fitting 실습 (2)**

---

이번 시간에는 실제 **제일원리계산(First-Principles Calculation)** 데이터에 **Birch-Murnaghan 상태방정식(Equation of State, EOS)**을 피팅하여 재료의 물성을 추출합니다.

> 💡 **DFT와 제일원리계산이란?**
>
> **밀도범함수이론(Density Functional Theory, DFT)**은 양자역학을 기반으로 원자 구조와 전자 상태를 계산하는 방법입니다. 실험 없이 컴퓨터 시뮬레이션만으로 재료의 에너지, 구조, 전자 구조 등 다양한 물성을 예측할 수 있습니다. 우리 연구실에서는 이러한 제일원리계산을 통해 새로운 소재의 물성을 탐색하고 설계합니다 ([tmd2.khu.ac.kr](http://tmd2.khu.ac.kr)).

---

## **📐 Birch-Murnaghan 상태방정식**

![EOS](https://bgjang-khu.github.io/MSE201/Ch6/data/EOS.png)

고체 재료에 압력을 가하면 부피가 변합니다. **Birch-Murnaghan EOS**는 부피 $V$에 따른 내부 에너지 $E(V)$를 기술하는 방정식으로, 재료의 탄성 물성을 나타내는 핵심 파라미터를 포함합니다.

$$E(V) = E_0 + \frac{9V_0B_0}{16}\left\{\left[\left(\frac{V_0}{V}\right)^{2/3}-1\right]^3 B_p + \left[\left(\frac{V_0}{V}\right)^{2/3}-1\right]^2\left[6-4\left(\frac{V_0}{V}\right)^{2/3}\right]\right\}$$

| 파라미터 | 설명 | 단위 |
|---|---|---|
| $E_0$ | 평형 부피에서의 내부 에너지 | eV |
| $V_0$ | 평형 부피 (equilibrium volume) | Å³ |
| $B_0$ | 체적탄성계수 (bulk modulus) — 재료의 압축 저항성 | eV/Å³ |
| $B_p$ | 압력에 대한 bulk modulus의 미분값 | 무차원 |

$B_0$가 클수록 압축하기 어려운 단단한 재료입니다. 단위 변환: $1 \text{ eV/Å}^3 = 160.2176 \text{ GPa}$

---

## **📂 실습 데이터**

DFT 계산으로 얻은 실리콘(Si)의 부피-에너지 데이터입니다. 여러 부피에서 에너지를 계산하여 $E(V)$ 곡선을 구성한 것입니다.

- [Si_EV.txt 다운로드](https://bgjang-khu.github.io/MSE201/Ch6/data/Si_EV.txt)

```
# Vol (angstrom^3)  ENE (eV)
139.00  -42.04309494
147.17  -42.85222052
155.35  -43.27474421
163.53  -43.39983667
171.70  -43.29652742
179.88  -43.01894597
```

---

## **🤔 왜 2단계로 피팅하나요?**

BM EOS는 파라미터가 4개이고 함수 형태가 복잡합니다. 그래서 먼저 **2차 함수로 간단하게 피팅**하여 $V_0$(에너지 최솟값 위치)와 $E_0$(최솟값)의 대략적인 위치를 파악하고, 이를 BM 피팅의 초기값으로 활용합니다.

```
2차 함수 피팅 → V0, E0 초기값 추정 → BM EOS 피팅 → 정확한 파라미터
```

---

## **함수 설계**

| 함수 | 입력 | 반환 | 역할 |
|---|---|---|---|
| `BM_EV(V, E0, V0, B0, Bp)` | V 배열, 파라미터 4개 | E 배열 | BM EOS 모델 함수 (제공) |
| `guess_fit(Vol, Ene)` | Vol, Ene 배열 | `popt1` | 2차 함수 피팅으로 초기값 추정 + 시각화 |
| `BM_fit(Vol, Ene, popt1)` | Vol, Ene 배열, popt1 | `popt2` | BM EOS 피팅 + 시각화 |

---

## **1. `BM_EV(V, E0, V0, B0, Bp)` — BM EOS 모델 함수 (제공)**

`curve_fit()`에 전달할 모델 함수입니다. 직접 구현할 필요 없이 그대로 사용합니다.

```python
import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt

def BM_EV(V, E0, V0, B0, Bp):
    eta = (V0 / V)**(2/3)
    return E0 + (9/16) * B0 * V0 * ((eta - 1)**3 * Bp + (eta - 1)**2 * (6 - 4 * eta))
```

---

## **2. `guess_fit(Vol, Ene)` — 2차 함수로 초기값 추정**

- **입력**: `Vol` — 부피 배열, `Ene` — 에너지 배열
- **반환**: `popt1` — 2차 함수 피팅 파라미터 (`[a, V0_guess, E0_guess]`)
- 2차 함수 $f(x) = a(x-b)^2 + c$ 로 피팅하여 에너지 최솟값 위치($V_0$)와 최솟값($E_0$)을 추정합니다.
- 데이터와 피팅 곡선을 그래프로 표시합니다.

```python
def quadratic(x, a, b, c):
    return a * (x - b)**2 + c
```

**출력 예시**

```
초기 추정값: V0 = 164.938 Å³,  E0 = -43.434 eV
```


---

## **3. `BM_fit(Vol, Ene, popt1)` — BM EOS 피팅**

- **입력**: `Vol` — 부피 배열, `Ene` — 에너지 배열, `popt1` — `guess_fit()`에서 반환된 초기값
- **반환**: `popt2` — BM 피팅 파라미터 (`[E0, V0, B0, Bp]`)
- `popt1`에서 $V_0$, $E_0$ 초기값을 꺼내 BM EOS 피팅의 초기값으로 사용합니다.
- `p0=[E0_guess, V0_guess, 0.5, 4.0]`으로 초기값을 설정합니다.
- 피팅 결과를 출력하고 그래프로 표시합니다.
- $B_0$는 eV/Å³ 단위이므로 GPa로 변환하여 출력합니다. ($\times 160.2176$)

**출력 예시**

```
E0 = -43.399594 eV
V0 = 163.57056 Å³
B0 = 88.633 GPa
Bp = 4.2502
```

---

## **🚀 전체 실행**

```python
data = np.loadtxt('Si_EV.txt', comments='#')
Vol  = data[:, 0]
Ene  = data[:, 1]

popt1 = guess_fit(Vol, Ene)
popt2 = BM_fit(Vol, Ene, popt1)
```



<details markdown="1">
<summary>예시 풀이</summary>

```python
import numpy as np
from scipy.optimize import curve_fit
import matplotlib.pyplot as plt

def BM_EV(V, E0, V0, B0, Bp):
    eta = (V0 / V)**(2/3)
    return E0 + (9/16) * B0 * V0 * ((eta - 1)**3 * Bp + (eta - 1)**2 * (6 - 4 * eta))

def quadratic(x, a, b, c):
    return a * (x - b)**2 + c

def guess_fit(Vol, Ene):
    popt1, pcov1 = curve_fit(quadratic, Vol, Ene)
    V0_guess = popt1[1]
    E0_guess = popt1[2]
    print(f'초기 추정값: V0 = {V0_guess:.3f} Å³,  E0 = {E0_guess:.3f} eV')

    Vfit = np.linspace(0.95*min(Vol), 1.05*max(Vol), 200)
    plt.figure(figsize=(8, 5))
    plt.scatter(Vol, Ene, c='blue', s=50, label='DFT data')
    plt.plot(Vfit, quadratic(Vfit, *popt1), 'r-', linewidth=2, label='Quadratic fit')
    plt.xlabel('Volume (Å³)')
    plt.ylabel('Energy (eV)')
    plt.title('Si: Initial Guess (Quadratic Fitting)')
    plt.legend()
    plt.grid(True)
    plt.show()

    return popt1

def BM_fit(Vol, Ene, popt1):
    V0_guess = popt1[1]
    E0_guess = popt1[2]

    popt2, pcov2 = curve_fit(BM_EV, Vol, Ene, p0=[E0_guess, V0_guess, 0.5, 4.0])
    E0, V0, B0, Bp = popt2

    print(f'E0 = {E0:.6f} eV')
    print(f'V0 = {V0:.5f} Å³')
    print(f'B0 = {B0 * 160.2176:.3f} GPa')
    print(f'Bp = {Bp:.4f}')

    Vfit = np.linspace(0.95*min(Vol), 1.05*max(Vol), 200)
    plt.figure(figsize=(8, 5))
    plt.scatter(Vol, Ene, c='blue', s=50, label='DFT data')
    plt.plot(Vfit, BM_EV(Vfit, *popt2), 'r-', linewidth=2, label='BM fitting')
    plt.xlabel('Volume (Å³)')
    plt.ylabel('Energy (eV)')
    plt.title('Si: Birch-Murnaghan EOS Fitting')
    plt.legend()
    plt.grid(True)
    plt.show()

    return popt2

data  = np.loadtxt('Si_EV.txt', comments='#')
Vol   = data[:, 0]
Ene   = data[:, 1]

popt1 = guess_fit(Vol, Ene)
popt2 = BM_fit(Vol, Ene, popt1)
```
</details>

