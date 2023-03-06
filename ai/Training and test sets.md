
## 지도 학습
- 훈련하기 위한 데이터와 정답이 필요
- 데이터는 `입력(input)`, 정답은 `타깃(target)`이라 하고, 이를 합쳐서 `훈련 데이터(training data)`라 함
- 지도 학습은 정답이 있으니 알고리즘이 정답을 맞히는 것을 학습하는 것


## 훈련 세트와 테스트 세트
- 평가에 사용하는 데이터를 `테스트 세트(test set)`, 훈련에 사용하는 데이터를 `훈련 세트(training set)`
- 훈련에 사용한 데이터로 모델을 평가하는 것은 적절하지 않음
- 훈련할 때 사용하지 않은 데이터로 평가하기 위해 훈련 데이터에서 일부를 떼어 테스트 세트로 사용함
- 도미와 빙어의 데이터를 합쳐 하나의 리스트로 준비
- 이 때 하나의 데이터를 `샘플(sample)`이라 함
```python
fish_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7, 31.0, 31.0, 31.5, 32.0, 32.0, 32.0, 33.0, 33.0, 33.5, 33.5, 34.0, 34.0, 34.5, 35.0, 35.0, 35.0, 35.0, 36.0, 36.0, 37.0, 38.5, 38.5, 39.5, 41.0, 41.0, 9.8, 10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2, 12.4, 13.0, 14.3, 15.0]

fish_weight = [242.0, 290.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0, 450.0, 500.0, 475.0, 500.0, 500.0, 340.0, 600.0, 600.0, 700.0, 700.0, 610.0, 650.0, 575.0, 685.0, 620.0, 680.0, 700.0, 725.0, 720.0, 714.0, 850.0, 1000.0, 920.0, 955.0, 925.0, 975.0, 950.0, 6.7, 7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2, 13.4, 12.2, 19.7, 19.9]

fish_data = [[l, w] for l, w in zip(fish_length, fish_weight)]
fish_target = [1]*35 + [0]*14

from sklearn.neighbors import KNeighborsClassifier

kn = KNeighborsClassifier()

train_input = fish_data[:35]
train_target = fish_target[:35]

test_input = fish_data[35:]
test_target = fish_target[35:]

kn.fit(train_input, train_target)
kn.score(test_input, test_target)
```
```
0.0
```



## 샘플링 편향
- 훈련 세트와 테스트 세트를 나누려면 데이터를 골고루 섞어야 함
- 골고루 섞여 있지 않으면 한쪽으로 치우져여 있다 하여 `샘플링 편향(sampling bias)`이라고 부름
- 넘파이(numpy) 사용
```python
import numpy as np

input_arr = np.array(fish_data)
target_arr = np.array(fish_target)

print(input_arr)
```
- `shape` 속성으로 배열의 크기를 알 수 있음
```python
print(input_arr.shape)     # 여기에선 (샘플 수, 특성 수) 출력
```
- `arange()`함수를 사용하여 0-48까지 1씩 증가하는 인덱스를 만들고 이를 랜덤으로 섞음
```python
np.random.seed(42)
index = np.arange(49)
np.random.shuffle(index)
```