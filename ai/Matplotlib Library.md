
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



