## Datime in Python

`Naive datetime`
* 타임존에 대한 정보는 없고 시간 정보만 담겨 있음

`Aware datetime`
* 타임존에 대한 정보인 tzinfo와 시간정보가 함께 담겨 있음


## USE_TZ

```python
from django.utils import timezone

# USE_TZ=True -> aware
now = timezone.now()

# USE_TZ=False -> naive
now = timezone.now()
```

`USE_TZ`
* 장고 내부적으로 **aware 객체를 사용할지 말지**에 대한 설정값으로, 기본값은 False.
* True로 활성화 하게되면 장고는 내부적으로 tzinfo가 담긴 timezone을 사용하게 됨
* DB에 저장할 때는 UTC 기준으로 저장하며, 사용자에게 보여질 때는 TIME_ZONE에 맞게 변환해서 표시됨

## TIME_ZONE

* 장고 서버(코드베이스)에서 인식하는 시간대값, 기본값은 UTC
* TIME_ZONE 값을 'Asia/Seoul'로 설정하고 USE_TZ=True를 설정하면 DB에서 값을 읽어오는 경우 자동으로 한국 시간대로 변경됨
