## 10진수 → X진수

```python
n = int(input())

def binary(n):
	digits = []
	
	while n != 0:
			digits.append(n % 2) # 10진수를 2진수로 바꾸는 과정이기떄문에 2로 계속 나누어줌
			n //= 2
	
	return digits[::-1] # 거꾸로 읽어줘야함!
```

## X진수 → 10진수

```python
def decimal(input_num, formation):
    lst = [int(digit) for digit in str(input_num)]

    answer = 0
    for i in range(len(lst)):
        answer = answer * formation + lst[i]
    
    return answer
```
