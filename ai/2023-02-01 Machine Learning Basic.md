
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


#### 1-2. 비선형 모델
- 직선이 아님
- 데이터를 어떻게 변경하더라도 선형으로 표현할 수 없는 모델

![비선형모델](nonlinear.png)


### 2. 비지도학습(Unsupervised Learning)
- 오직 `Input Data`를 기반으로 군집을 찾는 학습 진행
- 정답이 없는 학습을 모델에게 요구
- 데이터 분석을 통해 `unknown pattern`을 학습할 수 있음
	- 예를 들어 데이터 간의 유삳



