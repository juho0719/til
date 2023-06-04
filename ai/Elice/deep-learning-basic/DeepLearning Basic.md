
## 문제 6
```python
import tensorflow as tf
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
  
def insert():
	x = float(input('정수 또는 실수를 입력하세요. x : '))
	y = float(input('정수 또는 실수를 입력하세요. y : '))
	z = float(input('정수 또는 실수를 입력하세요. z : '))
	cal = input('어떤 연산을 할 것인지 입력하세요. (+-, **, *+, /-)')
	
	return x, y, z, cal
  
# 지시사항 1번을 참고하여 코드를 작성하세요.
def calcul(x, y, z, cal):
result = 0
	# +- 연산
	if(cal == '+-'):
		result = tf.subtract(tf.add(x,y), z)
	# ** 연산
	if(cal == '**'):
		result = tf.multiply(tf.multiply(x,y), z)
	# *+ 연산
	if(cal == '*+'):
		result = tf.add(tf.multiply(x,y), z)
	# /- 연산
	if(cal == '/-'):
		result = tf.subtract(tf.truediv(x,y), z)
	
	return result.numpy()

def main():
	x, y, z, cal = insert()
	print('\n연산 결과: ', calcul(x, y, z, cal))

if __name__ == "__main__":
	main()
```


## 문제 7
```python
import tensorflow as tf
import numpy as np
import random
import os
from plot import *
import warnings, logging, os
logging.disable(logging.WARNING)
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '3'
  
# 채점을 위한 코드입니다. 수정하지 마세요!
np.random.seed(81)
tf.random.set_seed(81)
  
def CNN():
	# 지시사항 1번을 참고하여 코드를 작성하세요.
	model = tf.keras.Sequential([
	tf.keras.layers.Conv2D(8, (3,3), strides=(1,1), padding='same', input_shape=(28,28,1)),
	tf.keras.layers.BatchNormalization(),
	tf.keras.layers.Activation('relu'),
	tf.keras.layers.Conv2D(16, (5,5), strides=(1,1), padding='same'),
	tf.keras.layers.BatchNormalization(),
	tf.keras.layers.Activation('relu'),
	tf.keras.layers.MaxPool2D(pool_size=(2,2)),
	tf.keras.layers.Conv2D(24, (3,3), strides=(1,1), padding='same'),
	tf.keras.layers.BatchNormalization(),
	tf.keras.layers.Activation('relu'),
	tf.keras.layers.MaxPool2D(pool_size=(2,2)),
	tf.keras.layers.Flatten(),
	tf.keras.layers.Dense(10, activation='softmax')
	])
	
	return model

def main():
	# 데이터를 불러옵니다.
	x_train = np.loadtxt('./data/train_images.csv', delimiter =',', dtype = np.float32)
	y_train = np.loadtxt('./data/train_labels.csv', delimiter =',', dtype = np.float32)
	x_test = np.loadtxt('./data/test_images.csv', delimiter =',', dtype = np.float32)
	y_test = np.loadtxt('./data/test_labels.csv', delimiter =',', dtype = np.float32)
	
	# 지시사항 2번을 참고하여 코드를 작성하세요.
	x_train = np.expand_dims(x_train.reshape(4000, 28, 28), -1)
	x_test = np.expand_dims(x_test.reshape(1000, 28, 28), -1)
	
	model = CNN()
	
	# 지시사항 3번을 참고하여 코드를 작성하세요.
	model.compile(loss='sparse_categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
	
	# 지시사항 4번을 참고하여 코드를 작성하세요.
	model.fit(x_train, y_train, epochs=10, batch_size=64, verbose=0)
	
	# 지시사항 5번을 참고하여 코드를 작성하세요.
	loss, test_acc = model.evaluate(x_test, y_test, verbose=0)
	
	predictions = model.predict_classes(x_test)
	print('\nTEST 정확도:', test_acc, '\n')
	
	# 완성된 모델을 확인해봅니다.
	model.summary()
	
	# 임의의 3가지 test data의 이미지와 실제 레이블 값을 출력하고 예측된 레이블 값을 출력합니다.
	plot(x_test, y_test, predictions)

if __name__ == "__main__":
	main()
```