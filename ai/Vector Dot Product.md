
### Dot
- `np.dot(a,b)` 와 `np.matmul(a,b)` : Numpy 벡터 `a`와 `b`의 내적을 출력
- 둘 다 벡터의 내적에서는 같은 결과물을 나타내지만 3차원 배열의 계산부터는 서로 다른 결과를 출력
```python
import numpy as np

# 벡터 초기화
A = np.array([3,1,-1])
# 3차원 축 설정
x_axis = np.array([1,0,0])
y_axis = np.array([0,1,0])
z_axis = np.array([0,0,1])

# x축과의 내적
A_proj_x = np.dot(A, x_axis)
# y축과의 내적
A_proj_y = np.dot(A, y_axis)
# z축과의 내적
A_proj_z = np.dot(A, z_axis)

# 벡터 A와 같은 값을 갖는지 확인
if (A == A_proj_x * x_axis + A_proj_y * y_axis + A_proj_z * z_axis).all:
	print('3차원 축으로 분해 완료')
else:
	print('3차원 축으로 분해 실패')
```
