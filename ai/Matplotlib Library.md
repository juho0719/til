
- 데이터 시각화 라이브러리
- 간단한 선 그래프부터 산점도, 히스토그램 등의 다양한 함수 제공


### Import
```python
import matplotlib.pyplot as plt
```


### 기본 그래프 그리기
- `plot()`함수는 입력한 데이터의 포인트들을 연결하여 직선 그래프로 출력
- 첫 번째 입력 데이터인 `["Seoul", "Paris", "Seattle"]`은 x축에 해당하는 각 포인트를 의미
- 위와 같이 문자열(string형)로 입력받은 경우에는 순서에 따라 x축에 입력
- 두 번째 입력 데이터인 `[30, 25, 55]`는 y축에 해당하는 각 포인트를 의미
-  `.xlabel()`: x축 label 명칭 설정
-  `.ylabel()`: y축 label 명칭 설정
-  `.title()`: 타이틀 명칭 설정
-  `.show()`: 그래프 출력
```python
plt.plot(["Seoul", "Paris", "Seattle"], [30, 25, 55])
plt.xlabel('City')
plt.ylabel('Response')
plt.title('Experiment Result')
plt.show()
```
![[Pasted image 20230408150934.png]]

### 산점도 그래프 그리기
- `scatter()`는 연결되지 않는 포인트들을 그대로 출력하고 싶을 때 사용
```python
plt.scatter([1, 3, 2], [0, -1, 2])
```
![[Pasted image 20230408151745.png]]


### 히스토그램 그래프 그리기
- `hist()`는 입력데이터 분포를 히스토그램으로 표현
```python
plt.hist([1, 3, 2, 2, 3, 3, 3, 1, 4, 8, 2, 6, 6], bins=None)
```
![[Pasted image 20230408151921.png]]

- `bins`는 분포를 몇 개로 나누어 출력할지에 대한 값
```python
plt.hist([1, 3, 2, 2, 3, 3, 3, 1, 4, 8, 2, 6, 6], bins=2)
```
![[Pasted image 20230408152016.png]]
- bins를 2로 설정하면 4.5를 기준으로 2개의 range 안에서 개수를 출력
- 데이터가 1부터 8까지의 범위로 정해져 있기에 범주를 2개로 나누기 위해서 (1+8)/2=4.5 기준이 설정


### 막대 그래프 그리기
- `bar()`함수는 x축 포인트에 따른 y축 값을 막대그래프로 그려주는 함수
```python
plt.bar(["Seoul", "Paris", "Seattle"], [30, 25, 55])
plt.xlabel('City')
plt.ylabel('Response')
plt.title('Experiment Result')
```
![[Pasted image 20230408152216.png]]


### 한 공간에 여러개 그래프 그리기
- 연속적으로 여러 개의 그래프 함수를 사용하고 마지막에 `.show()` 함수를 사용
```python
plt.plot([1, 2, 3], [1, 4, 9])
plt.plot([2, 3, 4], [5, 6, 7])
plt.bar([1,2,4],[2, 3, 4])
plt.xlabel('Sequence')
plt.ylabel('Time(secs)')
plt.title('Result')
plt.legend(['Elice', 'AI', 'ML'])
plt.show()
```
![[Pasted image 20230408152425.png]]

### 여러 결과 창 묶어서 출력하기
- `subplot()`을 사용
```python
x = np.random.rand(5)
y = np.random.rand(5)

plt.subplot(221)
plt.scatter(x, y, s=80, c='r', marker=">")

plt.subplot(222)
plt.scatter(x, y, s=80, c='b', marker=(5, 0))

verts = np.array([[-1, -1], [1, -1], [1, 1], [-1, -1]])
plt.subplot(223)
plt.scatter(x, y, s=80, c='k', marker=verts)

plt.subplot(224)
plt.scatter(x, y, s=80, c='y', marker=(5, 1))

plt.show()
```
![[Pasted image 20230408152619.png]]

- `subplot()`입력 값으로 3개의 자연수가 필요
- 위 예시에서 사용된 `subplot(221)`은 (2,2)행렬 출력창에서 1번째 출력창으로 설정하는 것을 의미

### fig, ax를 나누어 설정
```python
x = np.arange(10)

# 초기 figure와 축을 설정합니다.
fig, ax = plt.subplots()

# y = x 그래프를 그립니다. 따라서 x 데이터는 x, y 데이터도 x로 설정합니다.
# label은 'y=x', 마커는 'o', 마커 색깔은 'blue', 그래프의 선 스타일은 ':'로 설정
ax.plot(x, x, label='y=x', marker='o', color='blue', linestyle=':')

# y = x^2 그래프를 그립니다. 따라서 x 데이터는 x, y 데이터는 x**2으로 설정합니다.
# label은 'y=x^2', 마커는 '^', 마커 색깔은 'red', 그래프의 선 스타일은 '--'로 설정
ax.plot(x, x**2, label='y=x^2', marker='^', color='red', linestyle='--')

# 그래프 제목을 'Graph'
ax.set_title('Graph')

# x label은 'x', y label은 'y'
ax.set_xlabel('x')
ax.set_ylabel('y')

# x 범위는 0부터 10까지, y 범위는 0부터 100까지
ax.set_xlim(0, 10)
ax.set_ylim(0, 100)

# 범례의 위치는 'upper left'로 하고, 그림자 효과는 넣고, 테두리는 둥글게
ax.legend(loc='upper left', shadow=True, fancybox=True)

# figure를 "plot.png"라는 이름으로 저장하세요.
fig.savefig('plot.png')
```
![[Pasted image 20230408154005.png]]


### 다양한 그래프를 그리기 위한 함수/라이브러리

- axb개의 그래프를 그릴 수 있는 초기 데이터 값
```python
fig, axes = plt.subplots(a,b)
```

#### Scatter
- figure(0,0)의 위치에 `scatter`그래프 그리기
- `x` : x축 데이터
- `y` : y축 데이터
- `c` : 데이터 폰트 색깔 설정
- `s` : 데이터 픈트 크기 설정
- `alpha` : 데이터 폰트 투명도 설정
```python
colors = np.random.randint(0,100,500)
axes[0,0].scatter(x, y, c=colors, s=2, alpha=0.7)
```

#### Bar
- figure(0,1)위치에  `bar`그래프 그리기
- `x` : x축 데이터
- `y` : y축 데이터
```python
x = np.range(10)
axes[0,1].bar(x, y)
```

#### Multi-Bar
- figure(1,0)위치에 `bar`그래프 그리기
- x축은 `x_ax`, y축은 각각 x,y,z로 설정
- `set_xticks(x_ax)`는 x축 데이터를 병렬적으로 설정
- `set_xticklabels(['A', 'B', 'C'])`는 x축 label을 'A', 'B', 'C'로 설정
```python
x = np.array([3,2,1])
y = np.array([2,3,2])
z = np.array([1,3,4])
data1 = [x, y, z]
x_ax = np.arange(3)

for i in x_ax:
	axes[1,0].bar(x_ax, data1[i], bottom=np.sum(data1[:i], axis=0))

axes[1,0].set_xticks(x_ax)
axes[1,0].set_xticklabels(['A', 'B', 'C'])
```

#### Histogram
- figure(1,1)에 `Histogram`그래프 그리기
- 입력 데이터는 `data`, `Histogram`표현시 분할되는 개수는 50
```python
data = np.array(data_x)
axex[1,1].hist(data, 60)
```

