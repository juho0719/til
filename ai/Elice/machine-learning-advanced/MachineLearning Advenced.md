
## 문제 6
```python
import pandas as pd
import numpy as np
from sklearn.cluster import KMeans
from sklearn.datasets import load_wine
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

from elice_utils import EliceUtils
elice_utils = EliceUtils()

# 와인 데이터를 불러오고 데이터 프레임으로 만들어주는 함수

def load_data():
	wine = load_wine()
	wineDF = pd.DataFrame(data = wine.data, columns = wine.feature_names)
	wineDF['target'] = wine.target
	return wineDF

'''
1. K-Means 클러스터링을 수행하는 k_means_clus 함수를 구현합니다.
Step01. K-Means 객체를 불러옵니다.
	클러스터의 개수는 3,
	중심점 초기화는 랜덤,
	random_state는 200으로 설정합니다.
Step02. K-Means 클러스터링을 수행합니다.
	클러스터링은 정답이 없는 데이터를
	사용합니다. 따라서 drop 메소드를 이용해
	target 변수를 제거한 데이터를 학습시켜줍니다.
Step03. 클러스터링 결과 즉, 각 데이터가 속한 클러스터 중심점들의 label을 wine 데이터 프레임인 'wineDF'에 추가합니다.
labels_ 메소드를 이용하세요.
'''

def k_means_clus(wineDF):
	kmeans = KMeans(init='random', n_clusters=3, random_state=200)
	kmeans.fit(wineDF)
	wineDF['cluster'] = kmeans.labels_
	
	# 클러스터링 결과를 보기 위한 groupby 함수
	wine_result = wineDF.groupby(['target','cluster'])['malic_acid'].count()
	print(wine_result)
	return wine_result, wineDF

'''
2. 주성분 분석(PCA)을 통해 클러스터링 결과를 2차원으로 시각화해주는 함수 Visualize를 구현합니다.
Step01. PCA 객체를 불러옵니다.
	원 데이터를 2차원으로 차원 축소할 수 있도록
	n_components는 2로 설정합니다.
Step02. 주성분 분석을 수행합니다.
Step03. 주성분 분석으로 차원을 축소시킨 결과를 반환합니다.
'''
  
def Visualize(wineDF):
	pca = PCA(n_components=2)
	pca.fit(wineDF)
	pca_transformed = pca.transform(wineDF)
	  
	wineDF['pca_x'] = pca_transformed[:,0]
	wineDF['pca_y'] = pca_transformed[:,1]
	  
	# 클러스터링된 값이 0, 1, 2 인 경우, 인덱스 추출
	idx_0 = wineDF[wineDF['cluster'] == 0].index
	idx_1 = wineDF[wineDF['cluster'] == 1].index
	idx_2 = wineDF[wineDF['cluster'] == 2].index
	
	# 각 클러스터 인덱스의 pca_x, pca_y 값 추출 및 시각화
	fig, ax = plt.subplots()
	ax.scatter(x=wineDF.loc[idx_0, 'pca_x'], y= wineDF.loc[idx_0, 'pca_y'], marker = 'o')
	ax.scatter(x=wineDF.loc[idx_1, 'pca_x'], y= wineDF.loc[idx_1, 'pca_y'], marker = 's')
	ax.scatter(x=wineDF.loc[idx_2, 'pca_x'], y= wineDF.loc[idx_2, 'pca_y'], marker = '^')
	ax.set_title('K-means')
	ax.set_xlabel('PCA1')
	ax.set_ylabel('PCA2')
	fig.savefig("plot.png")
	elice_utils.send_image("plot.png")
	return pca_transformed
  
def main():
	wineDF = load_data()
	wine_result, wineDF = k_means_clus(wineDF)
	Visualize(wineDF)
	
if __name__ == "__main__":
	main()
```


## 문제7. 다중 선형 회귀 모델로 당뇨병 예측하기


```python
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.datasets import load_diabetes
  
"""
1. 사이킷런에 존재하는 데이터를 불러오고, 불러온 데이터를 학습용 데이터와 테스트용 데이터로 분리하여 반환하는 함수 load_data를 구현합니다.
Step01. 사이킷런에 존재하는 diabetes 데이터를 (X, y)의 형태로 불러옵니다.
Step02. 불러온 데이터를 학습용 데이터와 테스트용 데이터로 분리합니다.
	학습용 데이터로 전체 데이터의 70%를 사용하고,
	테스트용 데이터로 나머지 30%를 사용합니다.
	동일한 결과 확인을 위하여 random_state를
	100으로 설정합니다.
"""
  
def load_data():
	X, y = load_diabetes(return_X_y=True)
	print("데이터의 입력값(X)의 개수 :", X.shape[1])
	train_X, test_X, train_y, test_y = train_test_split(X, y, test_size=0.3, random_state=100)
	return train_X, test_X, train_y, test_y

"""
2. 다중 선형 회귀 모델을 불러오고, 불러온 모델을 학습용 데이터에 맞추어 학습시킨 후 해당 모델을 반환하는 함수 Multi_Regression을 구현합니다.
  
Step01. 사이킷런에 구현되어 있는 다중 선형 회귀 모델을 불러와 'multilinear'에 정의합니다.
Step02. 불러온 모델을 학습용 데이터에 맞춰 학습시킵니다.
"""

def Multi_Regression(train_X,train_y):
	multilinear = LinearRegression()
	multilinear.fit(train_X, train_y)
	return multilinear

"""
3. 구현한 회귀 모델의 설명력을 표현하는 R-squared 값을 반환하는 함수 R2를 구현합니다.
Numpy를 활용하세요.
"""

def R2(predicted, test_y):
	mean_y = np.mean(test_y)
	total_ss = np.sum((test_y - mean_y)**2)
	residual_ss = np.sum((test_y - predicted)**2)
	r2 = 1 - (residual_ss / total_ss)
	
	return r2

"""
4. 모델 학습 및 예측 결과 확인을 위한 main 함수를 완성합니다.
Step01. 데이터를 불러오는 함수를 이용해서 학습용 데이터와 테스트용 데이터를 불러옵니다.
Step02. 학습된 모델을 'model'에 정의하고, 테스트 데이터에 대한 예측을 수행 후 'predicted'에 정의합니다.
Step03. R-squared 값을 반환하는 함수를 이용해서 'R2_score'를 정의합니다.
Step04. 학습된 모델의 'beta_0'와 'beta_i'들을 각각 변수 'beta_0'와 'beta_i_list'에 저장합니다.
"""

  
def main():
	train_X, test_X, train_y, test_y = load_data()
	model = Multi_Regression(train_X, train_y)
	predicted = model.predict(test_X)
	R2_score = R2(predicted, test_y)
	print("\n R2_score : ", R2_score)
	
	beta_0 = model.intercept_
	beta_i_list = model.coef_
	print("\n beta_0 : ", beta_0)
	print("beta_i_list : ", beta_i_list)
	
	return predicted, beta_0, beta_i_list, R2_score

if __name__ == "__main__":
	main()
```
