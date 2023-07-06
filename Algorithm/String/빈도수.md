## Counter 모듈

> 리스트에서 가장 흔하게 등장하는 단어를 추출함

#### 1 ) 최빈값이 여러 개 + 최빈값 아무거나 하나만 필요

```python
def commonWord(self, paragraph: str, banned: List[str]) -> str:
    # 금지된 단어를 제외 + 문자가 아닌 것은 공백으로 변환 + 소문자로 변환
    words = [word for word in re.sub(r'[^\w]', '', paragraph).lower().split() if word not in banned]

    # 빈도수가 가장 높은 첫번째 단어 리턴
    counts = collections.Counter(words)
    return counts.most_common(1)[0][0]
```

#### 2 ) 최빈값이 여러 개 + 최빈값 여러개 모두 필요

```python
def commonWord(self, paragraph: str, banned: List[str]) -> str:
    words = [word for word in re.sub(r'[^\w]', '', paragraph).lower().split() if word not in banned]

    counts = collections.Counter(words)
    most_common = counts.most_common()
    max = order[0][1] # 첫번째 단어의 빈도수를 가져옴

    lst = []
    for count in most_common:
        if count[1] == max:
            modes.append(num[0])
    return modes
```

### 사용법

```python
from collections import Counter

colors = ['red', 'blue', 'red', 'green', 'blue', 'blue']

count = Counter(colors)
print(count)

most_common_list = count.most_common()
print(most_common_list)

most_common = count.most_common(1)
print(most_common)
```
* most_common()는 등장한 횟수를 내림차순으로 정리
* 등장한 횟수가 같은 수들은 임의의 순서로 정렬
* most_common(N)는 상위 N개의 빈도수를 리턴

#### 결과 

```python
Counter({'blue': 3, 'red': 2, 'green': 1})
[('blue', 3), ('red', 2), ('green', 1)]
[('blue', 3)]
```


