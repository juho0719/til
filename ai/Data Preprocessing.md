
### 도미, 빙어데이터 준비
```python
fish_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7, 31.0, 31.0, 31.5, 32.0, 32.0, 32.0, 33.0, 33.0, 33.5, 33.5, 34.0, 34.0, 34.5, 35.0, 35.0, 35.0, 35.0, 36.0, 36.0, 37.0, 38.5, 38.5, 39.5, 41.0, 41.0, 9.8, 10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2, 12.4, 13.0, 14.3, 15.0]

fish_weight = [242.0, 290.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0, 450.0, 500.0, 475.0, 500.0, 500.0, 340.0, 600.0, 600.0, 700.0, 700.0, 610.0, 650.0, 575.0, 685.0, 620.0, 680.0, 700.0, 725.0, 720.0, 714.0, 850.0, 1000.0, 920.0, 955.0, 925.0, 975.0, 950.0, 6.7, 7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2, 13.4, 12.2, 19.7, 19.9]

import numpy as np
fish_data = np.column_stack((fish_length, fish_weight))
fish_target = np.concatenate((np.ones(35), np.zeros(14)))

from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target = train_test_split(fish_data, fish_target, stratify=fish_target, random_state=42)
```

### 수상한 도미 한 마리
- 이전과 마찬가지로 k-최근접 이웃으로 훈련
```python
from sklearn.neighbors import KNeighborsClassifier
kn = KNeighborsClassifier()
kn.fit(train_input, train_target)
kn.score(test_input, test_target)
```
```
1.0
```
- 다른 도미데이터 예측
```python
print(kn.predict[[25, 150]])
```
```
[0.]
```
- 도미라고 나오지 않아 이 샘플을 다른 데이터와 함께 산점도로 그림
```python
import matplotlib.pyplot as plt

plt.scatter(train_input[:,0], train_input[:,1])
plt.scatter(25, 150, marker='^')
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
```
![[bream_scatter_and_another_bream_graph.png]]

- 최근접 이웃은 주변의 샘플 중 다수인 클래스를 예측으로 사용
- KNeighborsClassifier클래스는 주어진 샘플에서 가장 가까운 이웃을 찾아 주는 `kneighbors()`메소드를 제공
- KNeighborsClassifier클래스의 이웃 개수인 n_neighbors의 기본 값은 5이므로 5개의 이웃을 반환
```python
distances, indexes = kn.kneighbors([[25, 150]])
```
- `indexes`배열을 사용해 이웃 샘플을 따로 구분하여 그림
```python
plt.scatter(train_input[:,0], train_input[:,1])
plt.scatter(25, 150, marker='^')
plt.scatter(train_input[indexes,0], train_input[indexes,1], marker='D')
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
```
![[bream_smelt_5d_graph.png]]
- 1개만 도미고, 나머지는 빙어인 것을 확인

### 기준을 맞춰라
- x축과 y축의 범위가 달라 더 먼거리처럼 보이지만 가까운 거리
- x축의 범위를 동일하게 0~1000으로 맞춤
```python
plt.scatter(train_input[:,0], train_input[:,1])
plt.scatter(25, 150, marker='^')
plt.scatter(train_input[indexes,0], train_input[indexes,1], marker='D')
plt.xlim((0, 1000))
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
```
![[Pasted image 20230316220745.png]]

- x축은 가장 가까운 이웃을 찾는데 크게 영향을 못 미침
- 두 특성의 값이 놓인 범위가 매우 다른 경우 이를 두 특성의 `스케일(scale)`이 다르다고 함
- 알고리즘을 제대로 사용하려면 특성값을 일정한 기준으로 맞춰야 함
- 이런 작업을 `데이터 전처리(data processing)`이라 함
- 가장 널리 사용하는 전처리 방법 중 하나는 