
## import
```python
import numpy as np
```

## Numpy Array

#### array
```python
np.array([0, 1, 2, 3])
```
```
array([0, 1, 2, 3])
```

#### arange
- `arange(start,stop,step)` 함수는 `start`부터 `stop`전까지 `step`만큼 크기로 증가
```python
np.arange(1,8,2)    # 범위(1-8), 2씩 증가
```
```
array([1, 3, 5, 7])
```
- 인자가 1개인 경우 `start = 0, step = 1`로 default 설정
```python
np.arange(1,4)    # 범위(1-4)
```
```
array([1, 2, 3])
```

#### zeros
- `zeros()`는 선언하고 싶은 배열의 크기를 입력하여 0으로 채워진 배열을 선언
```python
np.zeros((3,2))    # (3,2) 크기 0 행렬
```
```
array([[0., 0.],
	   [0., 0.],
	   [0., 0.]])
```

#### ones
- `ones()`는 선언하고 싶은 배열의 크기를 입력하여 1로 채워진 배열을 선언
```python
np.ones((3,4))    # (3,4) 크기 1 배열
```
```
array([[1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.]])
```

#### random
- `random()`은 선언하고 싶은 배열의 크기를 입력하여 0이상 1미만의 랜덤 값을 갖는 배열을 선언
```python
np.random.random((2,3))    # (2,3) 크기의 랜덤 행렬
```


## 배열 정보 확인

#### shape
- 배열의 `row`와 `column`의 크기를 출력
```python
c = np.array([[1,2,3],[4,5,6]])
c.shape
```
```
(2, 3)
```

#### size
- 배열 성분들의 총 개수 출력
```python
c.size
```
```
6
```

#### dtype
- 배열의 성분들 타입
```python
c.dtype
```
```
dtype(`int64`)
```

#### 성분 확인
- 배열의 인덱스를 사용하여 성분을 확인
```python
c[0,0]
```
```
1
```


## 배열 형태 변경

#### reshape
- `reshape` 함수를 통하여 배열의 형태를 변경
```python
a = np.array([1,2,3,4])
a.reshape((2,2))
```
```
array([1,2],
	  [3,4])
```
- 아래와 같이도 가능
```python
a = np.reshape(a, (2,2))
```
- `row`의 개수에 상관없이 `column`의 수를 고정하고 싶다면 `row`인자에 -1을 입력
```python
a_col = a.reshape((-1,1))
```
```
array([[1],
	   [2],
	   [3],
	   [4]])
```
- 반대도 마찬가지


## 배열 자르고 붙이기

#### 배열 자르기
```python
np.array([[1,2,3],[4,5,6]])
```
```
array([[1,2,3]
	   [4,5,6]])
```
- 원래 행렬에서의 `column` 벡터를 뽑고 싶을 때 `row` 인덱스를 입력하는 부분에는 `:`, `column` 인덱스를 입력하는 부분에는 해당 `column` 인덱스를 입력
```python
a[:,0]    # column 벡터로 자르기
```
```
array([1,4])
```
```python
a[1,:]    # 마지막 column 벡터로 자르기
```
```
array([4,5,6])
```
- `배열[시작값:도착값,간격]`을 이용하여 잘라 표시할 수 있음
```python
a = np.array([[1,2,3,4,5,6],[7,8,9,10,11,12]])    # (2,6) 크기의 행렬 a 선언
```
```
array([1,2,3,4,5,6],
      [7,8,9,10,11,12])
```
- `배열[시작값:]`은 시작값 인덱스를 시작으로 배열의 마지막 인덱스까지 배열을 표시
```python
a[:,4:]
```
```
array([5,6],
	  [11,12])
```
