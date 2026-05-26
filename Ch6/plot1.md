---
layout: default
title: Matplotlib 기초
parent: Ch6. 데이터 시각화 및 분석
nav_order: 1
---

# Matplotlib 기초

---

데이터를 숫자로만 보면 전체적인 경향이나 패턴을 파악하기 어렵습니다. **Matplotlib**은 Python에서 가장 널리 쓰이는 시각화 라이브러리로, 데이터를 그래프로 표현하는 다양한 기능을 제공합니다.

```python
import numpy as np
import matplotlib.pyplot as plt
```

---

## **📈 기본 그래프 그리기**

`plt.plot(x, y)`는 x, y 값을 받아 선 그래프를 그립니다. 리스트나 NumPy 배열 모두 사용할 수 있으며, **`plt.show()`를 호출해야 화면에 출력**됩니다.

```python
x = [0, 1, 2, 3]
y = [0, 1, 4, 9]

plt.plot(x, y)
plt.show()
```

NumPy의 `linspace()`를 활용하면 훨씬 촘촘하고 부드러운 곡선을 그릴 수 있습니다.

```python
x = np.linspace(-5, 5, 100)

plt.plot(x, x**2)
plt.show()
```

### **🚀 TRY IT!**

`np.linspace(-3, 3, 50)`으로 x를 만들고, $y = x^3 - x$ 를 그려보세요.

```python
x = np.linspace(-3, 3, 50)

plt.plot(x,           )   # y = x^3 - x
plt.show()
```

---

## **🎨 스타일 지정**

`plt.plot(x, y, 'style')`의 세 번째 인자로 **색상, 마커, 선 종류**를 한 번에 지정할 수 있습니다.

```python
x = np.linspace(-5, 5, 30)

plt.plot(x, x**2, 'ro')     # 빨간색(r) 원형 마커(o)
plt.show()
```

```python
plt.plot(x, x**2, 'g:')     # 초록색(g) 점선(:)
plt.show()
```

```python
plt.plot(x, x**2, 'b-')     # 파란색(b) 실선(-)
plt.show()
```

여러 심볼을 조합할 수도 있습니다.

```python
plt.plot(x, x**2, 'm*-.')   # 마젠타(m) 별 마커(*) 점선(-.)
plt.show()
```

색상과 마커를 더 세밀하게 지정하려면 **키워드 인자** 방식을 사용할 수 있습니다.

```python
plt.plot(x, x**2, color='red', marker='o', linestyle='dashed')
plt.show()
```

주요 심볼 목록은 다음과 같습니다.

| 색상 | 심볼 | 마커 | 심볼 | 선 종류 | 심볼 |
|---|---|---|---|---|---|
| blue | `b` | circle | `o` | solid | `-` |
| green | `g` | square | `s` | dashed | `--` |
| red | `r` | diamond | `d` | dotted | `:` |
| cyan | `c` | star | `*` | dashdot | `-.` |
| magenta | `m` | point | `.` | | |
| black | `k` | x-mark | `x` | | |

### **🚀 TRY IT!**

`x = np.linspace(0, 2*np.pi, 50)`으로 x를 만들고, $y = \sin(x)$를 초록 점선(`'g:'`)으로 그려보세요.

```python
x = np.linspace(0, 2*np.pi, 50)

plt.plot(x,        ,        )   # sin(x), 초록 점선
plt.show()
```

### **🤔 Wait and Think!**

`plt.plot()`을 여러 번 호출하면 어떻게 될까요? 직접 실행해보세요.

```python
x = np.linspace(-5, 5, 30)

plt.plot(x, x**2, 'b-')
plt.plot(x, x**3, 'r*')
plt.show()
```

---

## **✨ 그래프 꾸미기**

제목, 축 이름, 범례, 범위, 격자를 추가하면 그래프가 훨씬 읽기 쉬워집니다.

```python
x = np.linspace(-5, 5, 100)

plt.plot(x, x**2, 'b-', label='quadratic')
plt.plot(x, x**3, 'r*', label='cubic')

plt.title('다항함수 그래프')     # 제목
plt.xlabel('x')                 # x축 이름
plt.ylabel('y')                 # y축 이름
plt.legend(loc=2)               # 범례 (왼쪽 위)
plt.xlim(-6, 6)                 # x축 범위
plt.ylim(-150, 150)             # y축 범위
plt.grid()                      # 격자

plt.show()
```

### **🔑 핵심 포인트: 범례 위치 (`loc`)**

`plt.legend(loc=숫자)` 또는 `plt.legend(loc='위치 문자열')`로 범례 위치를 지정합니다.

| 위치 | 코드 | 위치 | 코드 |
|---|---|---|---|
| best | 0 | center left | 6 |
| upper right | 1 | center right | 7 |
| **upper left** | **2** | lower center | 8 |
| lower left | 3 | upper center | 9 |
| lower right | 4 | center | 10 |

### **🚀 TRY IT!**

`x = np.linspace(0, 2*np.pi, 100)`으로 x를 만들고, $\sin(x)$와 $\cos(x)$를 서로 다른 색으로 그린 뒤 제목, 축 이름, 범례를 추가해보세요.

```python
x = np.linspace(0, 2*np.pi, 100)

plt.plot(x, np.sin(x),        , label='sin(x)')
plt.plot(x, np.cos(x),        , label='cos(x)')

plt.title(        )
plt.xlabel(       )
plt.ylabel(       )
plt.legend()
plt.grid()

plt.show()
```

### **💡 TIP**

`plt.figure(figsize=(10, 6))`를 `plt.plot()` 전에 호출하면 그래프 크기를 조절할 수 있습니다. `figsize=(가로, 세로)` 단위는 인치입니다.

```python
plt.figure(figsize=(10, 4))    # 넓고 납작한 그래프

x = np.linspace(0, 2*np.pi, 100)
plt.plot(x, np.sin(x))
plt.show()
```

---

## **🔵 산점도 그리기 (`plt.scatter()`)**

`plt.scatter(x, y)`는 데이터를 **점(dot)**으로 표시합니다. `plt.plot()`과 달리 점들을 선으로 연결하지 않아서, 개별 데이터의 분포를 볼 때 유용합니다.

```python
import random

x = [random.uniform(-1, 1) for i in range(100)]
y = [random.uniform(-1, 1) for i in range(100)]

plt.scatter(x, y)
plt.show()
```

{: .highlight }
> 💡 **TIP**
> `[random.uniform(-1, 1) for i in range(100)]`
>
> 이 코드는 `for`문을 한 줄로 압축한 **list comprehension**입니다. `range(100)`을 순회하면서 `random.uniform(-1, 1)`을 100번 호출해 결과를 리스트로 만듭니다. 풀어 쓰면 다음과 같습니다.
> ```python
> x = []
> for i in range(100):
>     x.append(random.uniform(-1, 1))
> ```

`c`로 색상, `s`로 점 크기를 지정할 수 있습니다.

```python
plt.scatter(x, y, c='red', s=10)   # 빨간색, 크기 10
plt.show()
```

여러 그룹의 점을 다른 색으로 표시할 수도 있습니다.

```python
x1 = [random.uniform(-1, 0) for i in range(50)]
y1 = [random.uniform(-1, 0) for i in range(50)]
x2 = [random.uniform(0, 1) for i in range(50)]
y2 = [random.uniform(0, 1) for i in range(50)]

plt.scatter(x1, y1, c='blue', s=10, label='Group A')
plt.scatter(x2, y2, c='red',  s=10, label='Group B')
plt.legend()
plt.show()
```

### **🚀 TRY IT!**

`random.uniform(-2, 2)`로 x, y 좌표 200개를 만들고, $x^2 + y^2 \leq 1$ 인 점은 빨간색, 아닌 점은 파란색으로 scatter plot을 그려보세요.

---

---

## **💾 그래프 저장하기**

`plt.savefig()`를 사용하면 그래프를 파일로 저장할 수 있습니다. `plt.show()` **전에** 호출해야 합니다.

```python
x = np.linspace(-5, 5, 100)

plt.plot(x, np.sin(x), 'g-')
plt.title('sin(x)')

plt.savefig('sin_plot.png')    # PNG로 저장
plt.show()
```

`pdf`, `jpeg`, `png` 등 다양한 형식을 지원합니다.

```python
plt.savefig('sin_plot.pdf')    # PDF로 저장
plt.savefig('sin_plot.jpeg')   # JPEG로 저장
```

### **🛑 주의: `savefig()`는 `show()` 전에!**

`plt.show()`를 먼저 호출하면 그래프가 화면에 출력된 후 초기화되어, 저장하면 **빈 그림**이 저장됩니다. 반드시 `savefig()` → `show()` 순서로 호출하세요.

```python
# ❌ 잘못된 순서
plt.plot(x, x**2)
plt.show()
plt.savefig('wrong.png')   # 빈 그림 저장됨!

# ✅ 올바른 순서
plt.plot(x, x**2)
plt.savefig('correct.png') # 먼저 저장
plt.show()
```

### **🚀 TRY IT!**

위에서 만든 $\sin(x)$, $\cos(x)$ 그래프를 `'myplot.png'`로 저장해보세요.

---

## **✍️ 연습 문제**

### **1. Si 상태밀도(DOS) 그리기 (`Si_PDOS.txt`)**

실리콘(Si)의 부분 상태밀도(Partial Density of States, PDOS) 데이터를 읽어 그래프를 그립니다.

- [Si_PDOS.txt 다운로드](https://bgjang-khu.github.io/MSE201/Ch6/data/Si_PDOS.txt)

파일은 다음 6개의 컬럼으로 구성되어 있습니다.

```
#Energy    s       py      pz      px      tot
-6.705   0.275   0.045   0.060   0.035   0.415
-6.695   0.283   0.047   0.062   0.036   0.428
...
```


### **목표**

- `s` 오비탈, `p` 오비탈 전체(`px + py + pz`), `Total DOS`를 한 그래프에 그립니다.
- x축 범위: `-10 ~ 5`, y축 범위: `0 ~ 0.8`
- 제목, 축 이름, 범례를 추가합니다.

### **출력 예시**

![Si DOS](https://bgjang-khu.github.io/MSE201/Ch6/data/Si_DOS.png)

### **풀이 조건**

- 함수 없이 스크립트로 작성합니다.
- `fill_between`은 사용하지 않고 `plt.plot()`으로만 그립니다.


<!--
<details markdown="1">
<summary>예시 풀이</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

data = np.loadtxt('Si_PDOS.txt', comments='#').T

ene = data[0]
s   = data[1]
py  = data[2]
pz  = data[3]
px  = data[4]
p   = px + py + pz
tot = data[5]

plt.plot(ene, s,   'r-', label='s orbital')
plt.plot(ene, p,   'b-', label='p orbital')
plt.plot(ene, tot, 'k-', label='Total', linewidth=2)

plt.xlim(-5, 5)
plt.ylim(0, 0.5)
plt.title('Si Density of States')
plt.xlabel('Energy (eV)')
plt.ylabel('DOS (a.u.)')
plt.legend(loc=0)
plt.show()
```
</details>
-->

---

### **2. 월별 평균 기온 그래프 (`temp2024.txt`)**

파일 입출력 실습에서 다뤘던 `temp2024.txt`를 다시 활용합니다. 파일을 읽어 월별 평균 기온을 계산하고, 그래프로 시각화합니다. 이번에는 **두 개의 함수**로 나누어 작성합니다.

- [temp2024.txt 다운로드](https://bgjang-khu.github.io/MSE201/Ch6/data/temp2024.txt)

### **목표**

- 함수 1 `calc_monthly_avg(filename)` — 파일을 읽어 월별 평균 기온을 계산하고, 월 리스트와 평균 기온 리스트를 반환합니다.
- 함수 2 `plot_monthly_avg(months, avgs)` — 함수 1에서 반환된 값을 받아 그래프를 그립니다.

### **함수 설계**

| 함수 | 입력 | 반환 |
|---|---|---|
| `calc_monthly_avg(filename)` | 파일명 | `months`, `avgs` (두 리스트) |
| `plot_monthly_avg(months, avgs)` | 월 리스트, 평균 기온 리스트 | 없음 |

### **출력 예시**

```
1월: -2.0C
2월: 1.9C
3월: 8.4C
4월: 13.5C
5월: 18.4C
6월: 23.4C
7월: 27.3C
8월: 26.1C
9월: 21.2C
10월: 14.8C
11월: 6.2C
12월: -0.4C
```

![Temp](https://bgjang-khu.github.io/MSE201/Ch6/data/Temp.png)

```python
import numpy as np
import matplotlib.pyplot as plt

def calc_monthly_avg(filename):
    f = open(filename, 'r', encoding='utf-8')

    months = []
    avgs   = []

    return months, avgs                        # 두 리스트 함께 반환


def plot_monthly_avg(months, avgs):



#### 실행 부분 ####

months, avgs = calc_monthly_avg('temp2024.txt')
plot_monthly_avg(months, avgs)
```

<!--
<details markdown="1">
<summary>예시 풀이</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

def calc_monthly_avg(filename):
    f = open(filename, 'r', encoding='utf-8')
    f.readline(); f.readline(); f.readline()

    months = []
    avgs   = []

    for month in range(1, 13):
        f.readline()
        temps = []

        while True:
            line = f.readline()
            if line.strip() == '':
                break
            temp = float(line.strip().split(':')[1][:-1])
            temps.append(temp)

        avg = sum(temps) / len(temps)
        months.append(month)
        avgs.append(avg)
        print(f'{month}월: {avg:.1f}C')

    f.close()
    return months, avgs


def plot_monthly_avg(months, avgs):
    plt.figure(figsize=(10, 5))
    plt.plot(months, avgs, 'bo-', linewidth=2, markersize=8)
    plt.title('2024 Monthly Average Temperature - KHU International Campus')
    plt.xlabel('Month')
    plt.ylabel('Temperature (C)')
    plt.xticks(months)
    plt.grid()
    plt.show()


#### 실행 부분 ####

months, avgs = calc_monthly_avg('temp2024.txt')
plot_monthly_avg(months, avgs)
```
</details>
-->
