## 정렬

```python
a = [2, 1, 5, 9, 7]
sorted(a) # [1, 2, 5, 7, 9]

b = 'zbdaf'
sorted(b) # ['a', 'b', 'd', 'f', 'z']
"".join(sorted(b)) # 'abdfz'

c = ['ccc', 'aaaa', 'd', 'bb']
sorted(c, key = len) # ['d', 'bb', 'ccc', 'aaaa']

d = ['cde', 'cfc', 'abc']
sorted(d, key=lambda s: s[0], s[-1]) # ['abc', 'cde', 'cfc']
```
