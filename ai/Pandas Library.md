
- Pandas는 row와 column으로 이뤄진 객체를 생성하고 수정하기 위한 패키지
- numpy는 수학적 계산을 위한 배열에 초점, pandas는 데이터 분석에 초점


### Import
```python
import pandas as pd
```

### Series
- 1차원 배열과 같은 자료구조
- 1차원 배열의 값뿐만 아니라 각 값에 해당하는 인덱스 값도 저장
```python
obj = pd.Series([4,7,-5])
```
```
0    4
1    7
2   -5
dtype: int64
```
- Series데이터는 파이썬 리스트와는 달리 인덱스값을 사용자가 설정 가능
- 인덱스가 같은 성분끼리 덧셈
```python
stock0 = pd.Series([10,20,30], index=['A','B','C'])
stock1 = pd.Series([30,20,10], index=['C','A','B'])
merge = stock0 + stock1
```
```
A    30
B    30
C    60
```

### Dataframe
- 여러 개의 Series가 모여서 행과 열을 이룬 데이터
- 'col0', 'col1', 'col2'는 column의 인덱스
- 따로 설정하지 않으면 자동으로 정수값으로 설정
```python
from pandas import DataFrame

raw_data = {'col0': [1,2,3,4],
		    'col1': [10,20,30,40],
		    'col2': [100,200,300,400]}

data = DataFrame(raw_data)
```
```
    col0  col1  col2
0     1    10    100
1     2    20    200
2     3    30    300
3     4    40    400
```
- DataFrame 객체 생성 시점에 row 인덱스와 column 인덱스 지정 가능
```python
a = [1,2], b = [3,4]
df = DataFrame(a, index=[0,1], columns=['a','b'])
```
```
    a    b
0   1    2
1   3    4
```

### Example

#### 5개의 age 데이터와 이름을 age로 선언
```python
import pandas as pd
age = pd.Series([10,20,40,50,60], name=)
```


