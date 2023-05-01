
```python
import numpy as np

# 리스트 안에 값들을 정규화 합니다.
def normalization(x):
  return [element / sum(x) for element in x]  

# 1. P(“스팸 메일”) 의 확률을 구하세요.
p_spam = 8 / 20

# 2. P(“확인” | “스팸 메일”) 의 확률을 구하세요.
p_confirm_spam = 5 / 8

# 3. P(“정상 메일”) 의 확률을 구하세요.
p_ham = 12 / 20

# 4. P(“확인” | "정상 메일" ) 의 확률을 구하세요.
p_confirm_ham = 2 / 12

# 내가 추가 한 것
p_confirm = (p_confirm_spam * p_spam) + (p_confirm_ham * p_ham)

# 5. P( "스팸 메일" | "확인" ) 의 확률을 구하세요.
p_spam_confirm = (p_confirm_spam*p_spam) / p_confirm

# 6. P( "정상 메일" | "확인" ) 의 확률을 구하세요.
p_ham_confirm = (p_confirm_ham*p_ham) / p_confirm

print("P(spam|confirm) = ",p_spam_confirm, "\nP(ham|confirm) = ",p_ham_confirm, "\n")

# 두 값을 비교하여 확인 키워드가 스팸에 가까운지 정상 메일에 가까운지 확인합니다.
value = [p_spam_confirm, p_ham_confirm]
result = normalization(value)

print("P(spam|confirm) normalization = ",result[0], "\nP(ham|confirm) normalization = ",result[1], "\n")

if p_spam_confirm > p_ham_confirm:
    print( round(result[0] * 100, 2), "% 의 확률로 스팸 메일에 가깝습니다.")
else :
    print( round(result[1] * 100, 2), "% 의 확률로 일반 메일에 가깝습니다.")
```


```python
import numpy as np
import elice_utils
import matplotlib.pyplot as plt
import matplotlib as mpl
mpl.use("Agg")
eu = elice_utils.EliceUtils()
learning_rate = 1e-4
iteration = 10000

x = np.array([[8.70153760], [3.90825773], [1.89362433], [3.28730045], [7.39333004], [2.98984649], [2.25757240], [9.84450732], [9.94589513], [5.48321616]])
y = np.array([[5.64413093], [3.75876583], [3.87233310], [4.40990425], [6.43845020], [4.02827829], [2.26105955], [7.15768995], [6.29097441], [5.19692852]])

##입력값(x)과 변수(a,b)를 바탕으로 예측값을 출력하는 함수를 만들어 봅니다.
def prediction(a,b,x):
    #1.Numpy 배열 a,b,x를 받아서 'x*(transposed)a + b'를 계산하는 식을 만듭니다.
    equation = np.dot(x, a) + b
    return equation

##변수(a,b)의 값을 어느정도 업데이트할 지를 정해주는 함수를 만들어 봅니다.
def update_ab(a,b,x,error,lr):
    ## a를 업데이트하는 규칙을 만듭니다..
    delta_a = -(lr*(2/len(error))*(np.dot(x.T, error)))
    ## b를 업데이트하는 규칙을 만듭니다.
    delta_b = -(lr*(2/len(error))*np.sum(error))
    
    return delta_a, delta_b

# 반복횟수만큼 오차(error)를 계산하고 a,b의 값을 변경하는 함수를 만들어 봅니다.
def gradient_descent(x, y, iters):
    ## 초기값 a= 0, b=0
    a = np.zeros((1,1))
    b = np.zeros((1,1))

    for i in range(iters):
        #2.실제 값 y와 prediction 함수를 통해 예측한 예측값의 차이를 error로 정의합니다.
        error = y - prediction(a,b,x)
        
        #3.위에서 정의한 함수를 이용하여 a와 b 값의 변화값을 저장합니다.
        a_delta, b_delta = update_ab(a,b,x,error,learning_rate)
        
        ##a와 b의 값을 변화시킵니다.
        a -= a_delta
        b -= b_delta

    return a, b

##그래프를 시각화하는 함수입니다.
def plotting_graph(x,y,a,b):
    y_pred=a[0,0]*x+b
    plt.scatter(x, y)
    plt.plot(x, y_pred)
    plt.savefig("test.png")
    eu.send_image("test.png")

##실제 진행 절차를 확인할 수 있는 main함수 입니다.
def main():
    a, b = gradient_descent(x, y, iters=iteration)
    print("a:",a, "b:",b)
    plotting_graph(x,y,a,b)
    return a, b

main()
```