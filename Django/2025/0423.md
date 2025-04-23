## rest_framework/serializers.py

```
class BaseSerializer(Field):
	def __init__(self, instance=None, data=empty, **kwargs)
    
class Serializer(BaseSerializer):
	pass
```

serializer.is_valid() 호출 시,
serializer.initial_data 에 data 값을 넣어주고
serializer.validated_data 를 통해 유효성 검증에 통과한 값들이 serializer.save() 시에 사용됩니다.
serializer.errors 에는 유효성 검증 후 오류 내역들이 저장되고
serializer.data 에는 유효성 검증 후 갱신된 인스턴스에 값이 사전형으로 저장됩니다.

```
def save(self, **kwargs):
	pass
```

serializer.save(**kwargs) 를 호출을 하면 DB에 저장한 관련 인스턴스를 리턴해주고
.validated_data 와 kwargs 사전을 합친 데이터를 DB로 저장하기 위한 시도를 합니다. 

저장 방식은 self.instance가 어떤가에 따라 다릅니다.
›› self.instance 인자를 지정했을 때 : .update() 를 통해 저장
›› self.instance 인자를 지정하지 않았을 때 : .create() 를 통해 저장

## Serializer Validators

UniqueValidator : 지정 1개 필드가 지정 QuerSet 범위에서의 유일성 여부 체크, 모델 필드에 unique=True를 지정하면 동작
UniqueTogetherValidator  
BaseUniqueForValidator  
UniqueForDateValidator (BaseUniqueForValidator) : 지정 날짜 범위에서 유일성 여부 체크  
UniqueForMonthValidator (BaseUniqueForValidator) : 지정 월 범위에서 유일성 여부 체크  
UniqueForYearValidator (BaseUniqueForValidator) : 지정 년 범위에서 유일성 여부 체크  

rest_framework.exceptions.ValidationError : 유효성 검사에 실패하면 ValidationError 예외를 발생

```
class ValidationError(APIException):
 	status_code = status.HTTP_400_BAD_REQUEST
 	default_detail = _('Invalid input.')
 	default_code = 'invalid'

 	def __init__(self, detail=None, code=None):
 		if detail is None:
   			detail = self.default_detail
 		if code is None:
  			code = self.default_code

 		if not isinstance(detail, dict) and not isinstance(detail, list):
   			detail = [detail]

   		self.detail = _get_error_details(detail, code)
```

## Serializer 에서 유효성 검사 함수 지정 

#### (1) Field Level 검사 : 필드 정의 시에 validator 지정

validator_필드명


```
from rest_framework import serializers
from rest_framework.exceptions import ValidationError

class PostSerializer(serializers.Serializser):
	title = serializers.CharField(max_length=100)
 
	def validte_title(self, value):
 		if '제목' not in value:
			raise ValidationError('제목에 필히 django 라는 말이 포함되어야합니다.')
  		return value
```

#### (2) Object Level 검사 : 클래스에 Meta.validator 지정

validate 함수를 사용해서 2개 이상의 필드에 대해 유효성 검사

```
class PostSerializer(serializer.Serializer):
	title = serializers.CharField(max_length=100)
    
 	def validate(self, data):
  		if '제목' not in value:
			raise ValidationError('제목에 필히 django 라는 말이 포함되어야합니다.')
  		return data
```
