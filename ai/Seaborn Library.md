
- `matplotlib`기반으로 다양한 색상 테마, 통계용 차트 기능이 추가된 패키지


### Import
- seaborn의 `load_dataset()` 또는 pandas의 `read_csv()`함수를 사용하여 데이터를 로드
- 붓꽃 데이터인 `iris`는 `load_dataset()`함수를 사용하여 불러올 수 있음
```python
import seaborn as sns

# iris(붓꽃) 데이터 불러오기
df = sns.load_dataset('iris')

# 전체 데이터에서 처음 5개의 row데이터 표시
df.head()
```
![[Pasted image 20230414082721.png]]


### 산점도와 분포 근사 선 그래프 그리기 - regplot()
- `regplot()`은 데이터의 위치인 산점도와 분포를 근사하는 선도 함께 출력할 수 있는 함수
- `regplot()` 파라미터
	- `x` : x축에 해당하는 데이터
	- `y` : y축에 해당하는 데이터
	- `color` : 색상을 설정
	- `line_kws` : 근사선의 특성 설정(Python Dictionary 형태)
	- `scatter_kws` : 데이터점들의 특성 설정 (Python Dictionary 형태)
- `sepal_length`와 `petal_length`는 꽃받침의 길이와 꽃잎의 길이 데이터를 의미
```python
import matplotlib.pyplot as plt

sns.regplot(x=df["sepal_length"], y=df["petal_length"])
plt.show()
```
![[Pasted image 20230414083258.png]]

- `color` 파라미터를 설정하여 색상 변경 가능
```python
sns.regplot(x=df["sepal_length"], y=df["petal_length"], color='red')
plt.show()
```
![[Pasted image 20230414083500.png]]

- `line_kws`로 근사선의 속성만 변경 가능
```python
sns.regplot(x=df["sepal_length"], y=df["petal_length"], line_kws={'color': 'red'})
plt.show()
```
![[Pasted image 20230414083615.png]]

- `scatter_kws`로 점의 속성만 변경 가능
```python
sns.regplot(x=df["sepal_length"], y=df["petal_length"], scatter_kws={'color': 'green'})
plt.show()
```
![[Pasted image 20230414083707.png]]


### 빈도수 막대그래프 그리기 - countplot()
- `countplot()`은 카테고리 값별로 빈도수를 표시하는 막대그래프를 출력
- `countplot()`의 대표적 파라미터
	- `x` : 열 이름(문자열)
	- `data` : 시각화를 위한 데이터 (Pandas DataFrame 형)
- seaborn에서 제공하는 타이타닉 탑승자 데이터를 가져와 "class"열의 빈도수를 출력
```python
import seaborn as sns
# 타이타닉 데이터 불러오기
titanic = sns.load_dataset("titanic") 
# 전체 데이터에서 처음 5개의 row 데이터 표시 (내용 확인)
titanic.head()
```
