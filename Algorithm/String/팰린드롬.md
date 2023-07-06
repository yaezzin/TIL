## 팰린드롬

주어진 문자열이 ```팰린드롬```인지 확인하기. 대소문자 구분하지 않으며, 영문자와 숫자만을 대상으로 함.

### 방법 1) 리스트로 변환

```python
def isPalindrome(self, s: str) -> bool:
    strs = []

    for s in str:
        if s.isalnum():
            strs.append(s.lower())

    while len(strs) > 1:
        if strs.pop(0) != strs.pop():
            return False

    return True
```
* ```isalnum()``` : 해당 문자가 숫자 또는 영어인지 판별

### 방법 2) 데크 자료형 이용

```python
def isPalindrome(self, s: str) -> bool:
    strs: Deque = collections.deque()

    for s in str:
        if s.isalnum():
            strs.append(s.lower())

    while len(strs) > 1:
        if strs.popleft() != strs.pop():
            return False

    return True
```
* pop(0)는 ```O(N)```의 시간이 걸리나 popleft()는 ```O(1)```의 시간이 걸림
* 따라서 리스트를 사용하면 ```O(N*N)```, 데크를 사용하면 ```O(1*N)```

### 방법 3 ) 정규식 + 슬라이싱 사용

```python
def isPalindrome(self, s: str) -> bool:
    s.lower()
    s = re.sub('^[a-z0-9]', '', s)
    return s == [s::-1]
```

