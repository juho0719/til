
```python
import re
import math
import numpy as np

def main():
	M1 = {'r': 0.7, 'g': 0.2, 'b': 0.1} # M1 기계의 사탕 비율
	M2 = {'r': 0.3, 'g': 0.4, 'b': 0.3} # M2 기계의 사탕 비율
	test = {'r': 4, 'g': 3, 'b': 3}
	
	print(naive_bayes(M1, M2, test, 0.7, 0.3))

def naive_bayes(M1, M2, test, M1_prior, M2_prior):
	p_x_m1 = (M1['r']**test['r']) * (M1['g']**test['g']) * (M1['b']**test['b'])
	p_m1_x = p_x_m1 * M1_prior
	p_x_m2 = (M2['r']**test['r']) * (M2['g']**test['g']) * (M2['b']**test['b'])
	p_m2_x = p_x_m2 * M2_prior
	
	p_test_m1 = p_m1_x / (p_m1_x + p_m2_x)
	p_test_m2 = p_m2_x / (p_m1_x + p_m2_x)
	
	return [p_test_m1, p_test_m2]


if __name__ == "__main__":
	main()
```