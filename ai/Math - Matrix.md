
### 행렬의 정의
- 행렬(matrix)은 수 또는 문자를 괄호안에 직사각형 형태로 배열한 것
- 머신러닝에선 각각의 샘플들을 다음과 같은 특징 벡터(feature vector)로 나타냄

##### 특징 벡터
​$$x=\begin{bmatrix}
0.2  \\ 
1.9  \\ 
\vdots   \\ 
5.3  \\ 
\end{bmatrix}$$

- 예를 들어 28x28크기를 가지는 흑백 이미지의 특징은 28x28 = 784개 
- 그에 따른 특징 벡터들을 모두 나열 후 어딘가에 사용하는 것은 좋지 방법이 아님
- 행렬의 경우 여러 개의 특징 벡터들을 담을 수 있고, 데이터를 간결하게 표현
- 후에 배울 딥러닝의 신경망 가중치 집합을 표현하거나 데이터의 상관관계를 나타내는 공분산을 표현할 때도 행렬 사용
- 따라서 머신러닝을 학습 하는 데 아주 중요

##### mxn 행렬
- mxn 행렬은 m개의 row와 n개의 column을 갖고 $a_i^j$ 의 성분으로 구성
$$A = \begin{bmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\ 
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots & \cdots & \vdots \\
a_{m,1} & a_{2,2} & \cdots & a_{m,n}
\end{bmatrix}$$


##### <예제 1> 다양한 shape의 numpy 행렬
```python
matrix_A = np.array([[1, 2],[3, 4]])
matrix_B = np.array([[5, 6],[7, 8]])
print('matrix_A: \n{}\n'.format(matrix_A))
print('matrix_B: \n{}'.format(matrix_B))
```
```
matrix_A: 
[[1 2]
 [3 4]]

matrix_B: 
[[5 6]
 [7 8]]
```

```python
matrix_C = np.array([[1, 2, 3],[4, 5, 6]])
matrix_D = np.array([[1, 2],[3, 4], [5, 6]])
print('matrix_C: \n{}\n'.format(matrix_C))
print('matrix_D: \n{}'.format(matrix_D))
```
```
matrix_C: 
[[1 2 3]
 [4 5 6]]

matrix_D: 
[[1 2]
 [3 4]
 [5 6]]
```

### 행렬 덧셈과 뺄셈, 스칼라 곱
- 머신러닝에서 입력된 데이터들은 행렬로 변환되어 사용

##### 행렬 덧셈
- 크기가 같은 두 행렬의 동일한 인덱스에 있는 행렬의 성분끼리 더함
$$\begin{bmatrix}
a_{1,1} & a_{1,2} \\ 
a_{2,1} & a_{2,2} 
\end{bmatrix} + 
\begin{bmatrix}
b_{1,1} & b_{1,2} \\ 
b_{2,1} & b_{2,2} 
\end{bmatrix} = 
\begin{bmatrix}
a_{1,1} + b_{1,1} & a_{1,2} + b_{1,2} \\ 
a_{2,1} + b_{2,1} & a_{2,2} + b_{2,2}
\end{bmatrix}$$

##### 행렬 뺄셈
- 크기가 같은 두 행렬의 동일한 인덱스에 있는 행렬의 성분끼리 뺄셈을 함
$$\begin{bmatrix}
a_{1,1} & a_{1,2} \\ 
a_{2,1} & a_{2,2} 
\end{bmatrix} - 
\begin{bmatrix}
b_{1,1} & b_{1,2} \\ 
b_{2,1} & b_{2,2} 
\end{bmatrix} = 
\begin{bmatrix}
a_{1,1} - b_{1,1} & a_{1,2} - b_{1,2} \\ 
a_{2,1} - b_{2,1} & a_{2,2} - b_{2,2}
\end{bmatrix}$$

##### 행렬 스칼라 곱
- 스칼라 값만큼 모든 성분에 곱
$$c\begin{bmatrix}
a_{1,1} & a_{1,2} \\ 
a_{2,1} & a_{2,2} 
\end{bmatrix} = 
\begin{bmatrix}
ca_{1,1} & ca_{1,2} \\ 
ca_{2,1} & ca_{2,2} 
\end{bmatrix}
$$

##### <예제 2> 행렬 덧셈과 뺄셈, 스칼라 곱
```python
matrix_A = np.array([[1, 2],[3, 4]])
matrix_B = np.array([[5, 6],[7, 8]])
print('matrix_A: \n{}\n'.format(matrix_A))
print('matrix_B: \n{}'.format(matrix_B))

print('matrix_A + matrix_B: \n{}\n'.format(matrix_A + matrix_B))
print('matrix_A - matrix_B: \n{}\n'.format(matrix_A - matrix_B))
print('2 X matrix_A: \n{}'.format(2*matrix_A))
```
```
matrix_A: 
[[1 2]
 [3 4]]

matrix_B: 
[[5 6]
 [7 8]]
matrix_A + matrix_B: 
[[ 6  8]
 [10 12]]

matrix_A - matrix_B: 
[[-4 -4]
 [-4 -4]]

2 X matrix_A: 
[[2 4]
 [6 8]]
```

### 행렬 곱셈
- 스칼라와 행렬과의 곱셈과는 달리 행렬 곱은 특수한 형태로 정의

##### 행렬 곱하기 벡터
$$\begin{bmatrix}
a_{1,1} & a_{1,2} \\ 
a_{2,1} & a_{2,2} 
\end{bmatrix}\begin{bmatrix}
x_{1}  \\ 
x_{2}  
\end{bmatrix} = \begin{bmatrix}
a_{1,1}  \\ 
a_{2,1}  
\end{bmatrix}*x_{1} + \begin{bmatrix}
a_{1,2}  \\ 
a_{2,2}  
\end{bmatrix}*x_{2} = \begin{bmatrix}
a_{1,1}x_{1}+a_{1,2}x_{2}  \\ 
a_{2,1}x_{1}+a_{2,2}x_{2}  
\end{bmatrix}$$

##### column 벡터 추가 형태의 행렬 곱하기 벡터
$$\begin{bmatrix}
a_{1,1} & a_{1,2} \\ 
a_{2,1} & a_{2,2} 
\end{bmatrix}\begin{bmatrix}
x_{1,1}  \\ 
x_{2,1}  
\end{bmatrix} =  \begin{bmatrix}
a_{1,1}x_{1,1}+a_{1,2}x_{2,1}  \\ 
a_{2,1}x_{1,1}+a_{2,2}x_{2,1}  
\end{bmatrix}$$

> $$\begin{bmatrix}
a_{1,1} & a_{1,2} \\ 
a_{2,1} & a_{2,2} 
\end{bmatrix}\begin{bmatrix}
x_{1,2}  \\ 
x_{2,2}  
\end{bmatrix} = \begin{bmatrix}
a_{1,1}x_{1,2}+a_{1,2}x_{2,2}  \\ 
a_{2,1}x_{1,2}+a_{2,2}x_{2,2}  
\end{bmatrix}$$

> $$\begin{bmatrix}
a_{1,1} & a_{1,2} \\ 
a_{2,1} & a_{2,2} 
\end{bmatrix}\begin{bmatrix}
x_{1,1} & x_{1,2} \\ 
x_{2,1} & x_{2,2}
\end{bmatrix} = \begin{bmatrix}
a_{1,1}x_{1,1}+a_{1,2}x_{2,1} & a_{1,1}x_{1,2}+a_{1,2}x_{2,2} \\ 
a_{2,1}x_{1,1}+a_{2,2}x_{2,1} & a_{1,1}x_{1,2}+a_{1,2}x_{2,2}
\end{bmatrix}$$

- 행렬 곱셈에서 중요한 것은 곱하는 행렬끼리 크기를 맞춰야 함
- 아래와 같은 계산은 불가

##### 행렬 곱셈이 불가능한 행렬 shape
- 곱하는 행렬의 column 개수와 뒤에 곱해지는 행렬의 row 개수가 같아야 함
- 행렬 곱셈은 결합 법칙은 성립되지만 교환 법칙이 성립하지 않음
- **결합 법칙**: $(AB)C=A(BC)$
- **교환 법칙**: $AB \neq BA$

##### <예제 3> 행렬 곱셈
