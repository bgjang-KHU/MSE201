---
layout: default
title: Environment Setup
nav_order: 2
---

# 프로그래밍 환경설정 

본격적인 수업에 앞서, Python 코딩을 위한 도구들을 설치해 봅시다.  
우리에게는 두 가지가 필요합니다. 첫 번째는 **Python 엔진**이고, 두 번째는 코드를 작성할 **편집기(Editor)**입니다.

편집기는 '메모장', '한글', '워드'와 같이 글을 작성하는데 도움을 주는 프로그램을 말합니다.
'메모장'으로도 문서를 작성하는데 문제가 없지만, '한글' 이나 '워드'를 사용하면 다양한 기능과 편하게 문서를 작성할 수 있습니다.
코딩도 마찬가지 입니다. '메모장'으로도 전혀 문제가 없지만, 'Jupyter notebook', 'VScode' 등 을 사용하면 더욱 편하게 코딩을 할 수 있습니다. 

---

## 1. 편집기(Editor)란 무엇인가요? ✍️

편집기는 '메모장', '한글', '워드'처럼 글을 작성하는 프로그램을 말합니다. 
'메모장'으로도 문서를 작성하는데 문제가 없지만, 우리는 더 예쁘고 편리하게 문서를 만들기 위해 '한글'이나 '워드'를 사용하죠.

코딩도 마찬가지입니다. 메모장으로도 코드를 짤 수는 있지만, 전용 편집기를 사용하면 다양한 기능과 함께 훨씬 편리하게 코드를 짤 수 있습니다.

---

## 2. 왜 **Thonny**인가요?

시중에는 `Jupyter Notebook`, `VS Code`, `PyCharm` 등 강력한 기능을 가진 편집기들이 많습니다. 하지만 초보자들에게는 너무 많은 기능이 오히려 큰 벽으로 느껴질 수 있습니다.

그래서 우리 수업에서는 **Thonny**라는 편집기를 사용합니다.
* **가볍고 단순합니다**: 복잡한 설정 없이 바로 코딩에 집중할 수 있습니다.
* **시각적입니다**: 프로그램이 돌아가는 과정을 눈으로 확인하기 좋습니다.
* **일체형입니다**: 파이썬 설치와 편집기 설치를 한 번에 해결할 수 있습니다.

---

## 3. 설치 가이드 (Step-by-Step)

### Step 1: Thonny 설치

1. [Thonny 공식 홈페이지](https://thonny.org/)에 접속합니다.
2. 우측 상단의 **Windows** (또는 Mac) 설치 파일을 다운로드합니다.
3. 설치 프로그램을 실행하여 기본 설정대로 설치를 완료합니다.

### Step 2: 필수 라이브러리 설치

우리 수업에서 수치 해석과 데이터 시각화를 위해 사용할 핵심 도구 4가지를 설치해 봅시다. Thonny에서는 클릭 몇 번으로 가능합니다.

1. **Thonny** 실행 후 상단 메뉴에서 **[Tools] → [Manage Packages]**를 클릭합니다.
2. 검색창에 아래 라이브러리 이름을 하나씩 검색하여 설치(Install) 버튼을 누릅니다.

* `numpy`: 수치 계산의 기초가 되는 도구
* `scipy`: 과학 기술 계산용 라이브러리
* `matplotlib`: 데이터를 그래프로 그려주는 도구
* `pandas`: 데이터 분석과 표(Table) 처리에 특화된 도구

### 💡 설치 확인하기 (중요!)

모든 설치가 끝났다면, 아래 코드를 Thonny 편집기 창에 복사해서 붙여넣고 [F5] 키를 눌러 실행해 보세요. 에러 없이 메시지가 출력된다면 준비는 모두 끝났습니다!

```python
import numpy as np
import scipy
import matplotlib
import pandas as pd

print("모든 라이브러리가 성공적으로 설치되었습니다!")
print(f"Numpy 버전: {np.__version__}")
print(f"Pandas 버전: {pd.__version__}")
```
{: .copyable }

```python
import numpy as np
from scipy.interpolate import griddata
import matplotlib.pyplot as plt
import numpy.ma as ma
from numpy.random import uniform, seed

# make up some randomly distributed data
seed(1234)
npts = 200
x = uniform(-2,2,npts)
y = uniform(-2,2,npts)
z = x*np.exp(-x**2-y**2)
# define grid.
xi = np.linspace(-2.1,2.1,100)
yi = np.linspace(-2.1,2.1,100)

# grid the data.
zi = griddata((x, y), z, (xi[None,:], yi[:,None]), method='cubic')
# contour the gridded data, plotting dots at the randomly spaced data points.
CS = plt.contour(xi,yi,zi,15,linewidths=0.5,colors='k')
CS = plt.contourf(xi,yi,zi,15,cmap=plt.cm.jet)
plt.show()
```
{: .copyable }