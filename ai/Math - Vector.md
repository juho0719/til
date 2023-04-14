
### 선형대수학
- 머신러닝에서 선형대수학이 없었다면 주어진 데이터를 한 줄씩 읽어와 계산해야 하고, 상관관계도 하나씩 비교하며 구해야 함
- 선형대수학은 행렬과 벡터를 사용해 수많은 데이터를 한 번에 묶어 계산을 매우 쉽게 도와주며 묶인 데이터의 고유 특징들을 알려줌으로써 다양한 특성들을 분석할 수 있게 함

### 스칼라와 벡터
- 스칼라(scalar) : 크기만 있고 방향은 가지지 않는 정보
- 벡터(vector) : 크기와 방향을 갖는 정보

##### 예제1 : x축과 y축 2차원 공간에서 A(빨강), B(파랑), C(초록) 벡터를 생성하고 시각화
```python
import numpy as np
import scipy as sc
import matplotlib.pyplot as plt

# A,B,C 벡터 정의
A = np.array([2,2])
B = np.array([-1,3])
C = np.array([5,2])
origin = [0], [0] # 원점

# 출력 파트
plt.quiver(*origin, [A[0],B[0],C[0]], [A[1],B[1],C[1]], color=['r','b','g'], angles='xy', scale_units='xy',scale=1)
plt.axis([-6,6,-6,6])
plt.grid(True)
plt.show()
```
![[Pasted image 20230414135544.png]]

##### 백터의 크기
- 벡터의 크기는 원점에서 벡터의 성분이 가리키고 있는 좌표를 화살표로 표시
- 화살표가 가리키는 좌표에서 x축으로 수선을 그으면 아래 그림과 같이 직각 삼각형을 만들 수 있음
![[Pasted image 20230414194226.png]]
- 벡터 $\vec{a}$의 크기는 직각삼각형의 빗변의 길이
- 이를 피타고라스의 정리를 사용하여 2차원 벡터의 크기를 구하면
$$|\vec{a}|=\sqrt{a_{1}^{2}+a_{2}^{2}}$$
- 2차원 이상의 벡터는 일반화하여 다음과 같이 구할 수 있음
$$|\vec{a}|=\sqrt{\sum_{i=1}^{n} a_{i}^{2}}, \;\;\;\; \vec{a}=[a_1,a_2,....,a_n]$$
- 벡터의 크기는 벡터의 `L2 norm`이라고 부르며, numpy에서 제공되는 `numpy.linalg.norm`함수를 이용해 벡터의 크기를 구할 수 있음


##### 예제 2 : 벡터의 크기
```python
from numpy import linalg as LA

# A,B,C 벡터 정의
A = np.array([3,4])
B = np.array([-1,3])
C = np.array([1,2,3,4])

A_norm = LA.norm(A)
B_norm = LA.norm(B)
C_norm = LA.norm(C)

print("A의 크기: {}".format(A_norm))
print("B의 크기: {}".format(B_norm))
print("C의 크기: {}".format(C_norm))
```
```
A의 크기 : 5.0
B의 크기 : 3.1622776601683795
C의 크기 : 5.477225575051661
```