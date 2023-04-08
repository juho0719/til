
- 데이터 시각화 라이브러리
- 간단한 선 그래프부터 산점도, 히스토그램 등의 다양한 함수 제공


### Import
```python
import matplotlib.pyplot as plt
```


### 기본 그래프 그리기
- `plot()`함수는 입력한 데이터의 포인트들을 연결하여 직선 그래프로 출력
```python
plt.plot(["Seoul", "Paris", "Seattle"], [30, 25, 55])
plt.xlabel('City')
plt.ylabel('Response')
plt.title('Experiment Result')
plt.show()
```
![[Pasted image 20230408150934.png]]

