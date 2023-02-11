
- 컴퓨터가 데이터를 바탕으로 학습하는 기술


## 머신러닝 종류
- 지도학습(Supervised Learning)
- 비지도학습(Unsupervised Learning)
- 준지도학습(Representation Learning)
- 강화학습(Reinforcement Learning)

### 1. 지도학습(Supervised Learning)
- 누군가가 정답을 가지고 지도해준 다는 의미로 지도학습이라 함
- 대부분이 생각하는 머신러닝이 지도학습
- `Input Data`와 정답에 해당하는 `Label(또는 Class)`정보를 입력 받음
	- `Input Data`로 자동차 사진을 자동차로 학습, 그 후 testing으로 사진을 보고 자동차가 맞는지 Y/N으로 말함
- 선형 모델과 비선형 모델로 나눔

#### 1-1. 선형모델
- 크게 분류와 회귀로 나눔
- 분류(Classification)
	- 타겟변수 Y가 이산형(discrete)이나 범주형(categorical)일때 사용
- 회귀(Regression)
	- 타겟변수 Y가 연속형(continuous)나 실수(real number)일때 사용
![선형모델](classification_regression.png)

##### 1-1-1. Lineer Regression

- Simple Lineer Regression Analysis
	- `y = wx + b`
	- `w`는 가중치(weight), `b`는 편향(basis)
	- `w`, `b`에 따라 `x`, `y`가 표현하는 직선은 무궁무진해 짐

- Multi Lineer Regression Analysis
	- `y = w1x1 + w2x2 + w3x3 ... wnxn + b`
	- `y`는 하나지만 `x`가 여러개

- 가설(Hypothesis) 세우기 
시간에 따라 다음과 같은 점수를 얻었다는 데이터가 있음
|hours(x)|score(y)|
|--------|--------|	
|2|25|
|3|50|
|4|42|
|5|61|
- x, y의 관계를 유추하기 위해 수학적으로 식을 세워보는 데 이를 `가설(Hypothesis)`라 함

- 비용 함수(Cost Function)
	- 가설을 세우고 이를 해결하기 위한 문제에 대한 규칙을 잘 표현한 `w`, `b`를 찾는 일





#### 1-2. 비선형 모델
- 직선이 아님
- 데이터를 어떻게 변경하더라도 선형으로 표현할 수 없는 모델

![비선형모델](nonlinear.png)


### 2. 비지도학습(Unsupervised Learning)
- 오직 `Input Data`를 기반으로 군집을 찾는 학습 진행
- 정답이 없는 학습을 모델에게 요구
- 데이터 분석을 통해 `unknown pattern`을 학습할 수 있음
	- 예를 들어 데이터 간의 유사도(similarity), 패턴(pattern), 차이(difference)등으로 데이터를 분류, 학습을 진행





