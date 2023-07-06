## 문자열 뒤집기

### 방법 1) 파이썬다운 방식

```python
def reverseString(self, s: List[str]) -> None:
    s.reverse()
```

### 방법 2) 투포인터

```python
def reverseString(self, s: List[str]) -> None:
    left, right = 0, len(s)-1

    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
         
```
