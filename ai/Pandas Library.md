
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
```tpy
```

