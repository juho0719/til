
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

```