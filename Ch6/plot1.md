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
