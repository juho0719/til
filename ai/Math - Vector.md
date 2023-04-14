
### 선형대수학
- 머신러닝에서 선형대수학이 없었다면 주어진 데이터를 한 줄씩 읽어와 계산해야 하고, 상관관계도 하나씩 비교하며 구해야 함
- 선형대수학은 행렬과 벡터를 사용해 수많은 데이터를 한 번에 묶어 계산을 매우 쉽게 도와주며 묶인 데이터의 고유 특징들을 알려줌으로써 다양한 특성들을 분석할 수 있게 함

### 스칼라와 벡터
- 스칼라(scalar) : 크기만 있고 방향은 가지지 않는 정보
- 벡터(vector) : 크기와 방향을 갖는 정보

##### <예제 1> x축과 y축 2차원 공간에서 A(빨강), B(파랑), C(초록) 벡터를 생성하고 시각화
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
- 이를 피타고라스의 정리를 사용

##### 2차원 벡터의 크기
$$|\vec{a}|=\sqrt{a_{1}^{2}+a_{2}^{2}}$$
- 2차원 이상의 벡터는 아래와 같이 일반화 하여 구할 수 있음

##### 벡터의 크기
$$|\vec{a}|=\sqrt{\sum_{i=1}^{n} a_{i}^{2}}, \;\;\;\; \vec{a}=[a_1,a_2,....,a_n]$$
- 벡터의 크기는 벡터의 `L2 norm`이라고 부르며, numpy에서 제공되는 `numpy.linalg.norm`함수를 이용해 벡터의 크기를 구할 수 있음


##### <예제 2> 벡터의 크기
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


### 벡터의 연산법
- 벡터 연산은 대표적으로 덧셈, 뺄셋, 스칼라와의 곱셉, 내적이 존재
- 벡터간의 덧셈과 뺄셈은 같은 인덱스에 있는 성분끼리 더하고 빼는 것으로 구할 수 있음
- 만약 두 벡터 $[a_{1},a_{2}]$ 와 $[b_{1},b_{2}]$ 를 더한다면 아래와 같이 연산

##### 벡터의 덧셈
$$[a_{1},a_{2}]+[b_{1},b_{2}]=[a_{1}+b_{1},a_{2}+b_{2}]$$

##### 벡터의 뺄셈
$$[a_{1},a_{2}]-[b_{1},b_{2}]=[a_{1}-b_{1},a_{2}-b_{2}]$$

##### <예제 3> 벡터의 뎃셈 구현
```python
A = np.array([2,2])
B = np.array([-1,3])
C = A + B

print("A + B = C -> {} + {} = {}".format(A,B,C))

plt.quiver(*origin, [A[0],B[0],C[0]], [A[1],B[1],C[1]], color=['r','b','g'], angles='xy', scale_units='xy',scale=1)
plt.axis([-6,6,-6,6])
plt.grid(True)
plt.show()
```
![[Pasted image 20230414200245.png]]

##### <예제 4> 벡터의 뺄셈 구현
```python
A = np.array([2,2])
B = np.array([-1,3])
C = A - B

print("A - B = C -> {} + {} = {}".format(A,B,C))

plt.quiver(*origin, [A[0],B[0],C[0]], [A[1],B[1],C[1]], color=['r','b','g'], angles='xy', scale_units='xy',scale=1)
plt.axis([-6,6,-6,6])
plt.grid(True)
plt.show()
```
![[Pasted image 20230414200329.png]]

##### 벡터와 스칼라의 곱셈
- 벡터의 모든 성분에 스칼라 값을 곱하여 구할 수 있음
- 스칼라 $c$ 와 벡터 $[a_1,a_2]$ 를 곱하면
$$c[a_{1},a_{2}]=[ca_{1},ca_{2}]$$

##### <예제 5> 벡터와 스칼라의 곱셈
```python
C = 2 * A

plt.quiver(*origin, [A[0],B[0],C[0]], [A[1],B[1],C[1]], color=['r','b','g'], angles='xy', scale_units='xy',scale=1)
plt.axis([-6,6,-6,6])
plt.grid(True)
plt.show()
```
![[Pasted image 20230414200643.png]]
- 붉은선(A)와 초록선(C)이 같은 방향에 있는 것을 보면 벡터에 스칼라를 곱해주면 크기는 변하지만 방향은 변함이 없다는 것을 알 수 있음

##### 벡터의 내적
- 벡터 간에는 곱셈이 정의되지 않고 `벡터 내적(Inner product or dot product)`이란 유사한 계산법이 존재
$$\vec{a}\cdot \vec{b}= |\vec{a}||\vec{b}|\cos(\theta)$$
- 벡터 내적의 정의로는 위와 같이 정리되지만, 아래와 같은 계산 방법이 더 자주 사용

##### 벡터 내적 계산법
$$\vec{a}\cdot \vec{b} = \sum_{i=1}^{n}a_{i}b_{i}, \;\; \;\; \vec{a}=[a_{1}, a_{2}, ..., a_{n}], \;\; \vec{b}=[b_{1}, b_{2}, ..., b_{n}]$$

##### 정사영
- 정사영(Orthographic projection)은 일반적으로 아래 그림과 같이 도형의 각 점에서 한 평면에 내린 수선의 발이 그리는 도형
![[Pasted image 20230414201041.png]]
- 영벡터가 아닌 두 벡터 $\vec{a}, \vec{b}$가 아래와 같이 공간상에 있다고 가정
![[Pasted image 20230414201117.png]]
- 벡터 $\vec{a}$를 벡터 $\vec{b}$ 기준으로 수선의 발을 내린다면 위 그림과 같이 표현
- $\vec{c}$ 벡터를 벡터 사영이라 부르고 그 크기는 벡터 $\vec{a}$와 벡터 $\vec{b}$의 내적인 $|\vec{a}|\cos(\theta)$으로 구할 수 있음
- 벡터 사영 $\vec{c}$는 $\vec{b}$ 방향의 $|\vec{a}|\cos(\theta)$ 크기를 갖기에 다음과 같이 표현 가능

##### 정사영 벡터 정의
$$\vec{c}=\frac{\vec{b}}{|\vec{b}|}|\vec{a}|\cos(\theta)$$

- 벡터 $\vec{c}$를 구하기 위해서 벡터 $\vec{a}, \vec{b}$의 그 사잇각 $\theta$를 계산하기는 쉽지 않음
- $\theta$를 구하지 않고 벡터의 내적을 활용하여 쉽게 $\vec{c}$를 구할 수 있음

##### 내적을 사용한 정사영 표현
$$\vec{c}=\frac{\vec{b}}{|\vec{b}||\vec{b}|}|\vec{a}||\vec{b}|\cos(\theta)=\frac{\vec{b}}{\vec{b}\cdot \vec{b}}\vec{a}\cdot \vec{b}=\frac{\vec{a}\cdot \vec{b}}{\vec{b}\cdot \vec{b}}\vec{b}$$

##### 머신러닝 기법에서의 내적
- 벡터의 내적은 벡터로 인코딩된 데이터 간의 유사도를 계산하기 위해 사용
- 벡터 간의 유사도를 계산하는 방법 중 대표적인 것이 `코사인 유사도(cosine similarity)` 측정 방법
- 코사인 유사도 측정 방법은 두 벡터 $\vec{a}, \vec{b}$가 이루는 사잇각 $\theta$의 코사인값을 보고 두 벡터, 즉 두 데이터가 얼마나 닮아있는지를 확인

##### 코사인 유사도

$cosine-similarity (\vec{a},  \vec{b}) = $
