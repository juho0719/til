
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
	- x : x축에 해당하는 데이터
	- y : y축에 해당하는 데이터
	- color : 색상을 설정
	- line_kws : 근사선의 특성 설정(Python Dictionary)