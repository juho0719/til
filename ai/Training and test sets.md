
## 지도 학습
- 훈련하기 위한 데이터와 정답이 필요
- 데이터는 `입력(input)`, 정답은 `타깃(target)`이라 하고, 이를 합쳐서 `훈련 데이터(training data)`라 함
- 지도 학습은 정답이 있으니 알고리즘이 정답을 맞히는 것을 학습하는 것


## 훈련 세트와 테스트 세트
- 평가에 사용하는 데이터를 `테스트 세트(test set)`, 훈련에 사용하는 데이터를 `훈련 세트(training set)`
- 훈련에 사용한 데이터로 모델을 평가하는 것은 적절하지 않음
- 훈련할 때 사용하지 않은 데이터로 평가하기 위해 훈련 데이터에서 일부를 떼어 테스트 세트로 사용함
- 