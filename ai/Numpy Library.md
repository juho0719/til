
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

