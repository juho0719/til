
### 선형대수학
- 머신러닝에서 선형대수학이 없었다면 주어진 데이터를 한 줄씩 읽어와 계산해야 하고, 상관관계도 하나씩 비교하며 구해야 함
- 선형대수학은 행렬과 벡터를 사용해 수많은 데이터를 한 번에 묶어 계산을 매우 쉽게 도와주며 묶인 데이터의 고유 특징들을 알려줌으로써 다양한 특성들을 분석할 수 있게 함

### 스칼라와 벡터
- 스칼라(scalar) : 크기만 있고 방향은 가지지 않는 정보
- 벡터(vector) : 크기와 방향을 갖는 정보
- 예제1 : x축과 y축 2차원 공간에서 A(빨강), B(파랑), C(초록) 벡터를 생성하고 시각화
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

