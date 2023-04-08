
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
age = pd.Series([10,20,40,50,60], name='age')
```

#### 파이썬 Dictionary형 데이터 class_name을 Series형 데이터로 생성
```python
class_name = ['국어' : 90, '영어' : 70, '수학' : 100, '과학' : 80]
class_series = pd.Series(class_name)
```

#### 2차원 데이터로 DataFrame 생성. index와 columns값을 설정
```python
df = pd.DataFrame([10,20],[30,40], index=['국어','영어'], columns=['철수','영희'])
```
```
      철수    영희
국어    10     20
영어    30     40
```



## Pandas 데이터 다루기

### 인덱스 & 슬라이싱
- age, weight, height의 Series를 DataFrame형태로 생성
```python
a = pd.Series([20, 15, 30, 25, 35], name='age')
b = pd.Series([68.5, 60.3, 53.4, 74.1, 80.7], name='weight')
c = pd.Series([180, 165, 155, 178, 185], name ='height')
human = pd.DataFrame([a,b,c])
```
```
            0      1      2      3      4
age      20.0   15.0   30.0   25.0   35.0
weight   68.5   60.3   53.4   74.1   80.7
height  180.0  165.0  155.0  178.0  185.0 
```

- `loc()`는 명시적 인덱스를 참조하는 인덱싱/슬라이싱
```python
human.loc['age']
```
```
0    20.0
1    15.0
2    30.0
3    25.0
```
```python
human.loc['weight','height']
```
```
            0      1      2      3      4
weight   68.5   60.3   53.4   74.1   80.7
height  180.0  165.0  155.0  178.0  185.0
```
- `iloc()`은 정수 인덱스/슬라이싱. 단, 리스트와 같이 마지막 인덱스는 포함하지 않음
```python
human.iloc[0]
```
```
0    20.0
1    15.0
2    30.0
3    25.0
```
```python
human.iloc[1:3]
```
```
            0      1      2      3      4
weight   68.5   60.3   53.4   74.1   80.7
height  180.0  165.0  155.0  178.0  185.0
```
- 새로운 데이터를 추가하려면 `DataFraem['인덱스'] = 배열`형태로 지정
```python
sex = ['F','M','F','M','F']
human['sex'] = sex
```
```
           0     1     2     3     4
age       20    15    30    25    35
weight  68.5  60.3  53.4  74.1  80.7
height   180   165   155   178   185
sex        F     M     F     M     F 
```
- `drop()`은 인덱스 및 column을 삭제
```python
human.drop(['height'])
```
```
           0     1     2     3     4
age       20    15    30    25    35
weight  68.5  60.3  53.4  74.1  80.7
sex        F     M     F     M     F 
```
