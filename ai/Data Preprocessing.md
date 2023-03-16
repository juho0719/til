
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
- 가장 널리 사용하는 전처리 방법 중 하나는 `표준점수(standard score)`(혹은 z점수라 부름)
	- 분산 : 데이터에서 평균을 뺀 값을 모두 제곱한 다음 평균을 내어 구함
	- 표준편차 : 분산의 제곱근으로 데이터가 분산된 정도를 나타냄
	- 표준점수 : 각 데이터가 원점에서 몇 표준편차만큼 떨여져 있는 지를 나타내는 값
- 표준점수를 계산하는 방법은 평균을 빼고 표준편차를 나누어 주면 됨
```python
mean = np.mean(train_input, axis=0)
std = np.std(train_input, axis=0)
```
- `np.mean()`함수는 평균으 ㄹ계산
- `np.std()`함수는 표준편차를 계산
- `train_input`은 (36,2)크기의 배열


```python
train_scaled = (train_input - mean) / std
```
- `train_input`의 모든 행에서 `mean`에 있는 두 평균값을 빼주고 `std`에 있는 두 표준편차를 다시 모든 행에 적용
- 이런 기능을 `브로드캐스팅(broadcasting)`이라 부름


### 전처리 데이터로 모델 훈련하기
- 표준점수로 변환한 `train_scaled`로 산점도를 그려보면
```python

```
![[Pasted image 20230316221828.png]]
- 훈련 세트를 `mean`(평균)으로 빼고 `std`(표준편차)로 나누어 주었기 때문에 값의 범위가 크게 달라짐
- 훈련세트의 `mean`과 `std`를 이용해서 변환해야 함
```python
new = ([25, 150] - mean) / std

plt.scatter(train_scaled[:,0], train_scaled[:,1])
plt.scatter(new[0], new[1], marker='^')
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
```
![[Pasted image 20230316222115.png]]

- k-최근법 이웃 모델
- 테스트 세트도 훈련 세트의 평균과 표준편차로 변환해야 함
- 그렇지 않다면 데이터의 스케일이 같아지지 않으므로 훈련한 모델이 쓸모 없어 짐
- 모델을 평가해보면
```python
test_scaled = (test_input - mean) / std
kn.score(test_scaled, test_target)
print(kn.predict([new]))
```

