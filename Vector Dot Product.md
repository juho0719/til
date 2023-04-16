
### Dot
- `np.dot(a,b)` 와 `np.matmul(a,b)` : Numpy 벡터 `a`와 `b`의 내적을 출력
- 둘 다 벡터의 내적에서는 같은 결과물을 나타내지만 3차원 배열의 계산부터는 서로 다른 결과를 출력
```python
import numpy as np

# 벡터 초기화
A = np.array([3,1,-1])
# 3차원 축 설정
x_axis = None
y_axis = None
z_axis = None

# x축과의 내적
A_proj_x = None
# y축과의 내적
A_proj_y = None
# z축과의 내적
A_proj_z = None
# 벡터 A와 같은 값을 갖는지 확인
if (A == A_proj_x * x_axis + A_proj_y * y_axis + A_proj_z * z_axis).all:
	print('3차원 축으로 분해 완료')
else:
	print('3차원 축으로 분해 실패')
```