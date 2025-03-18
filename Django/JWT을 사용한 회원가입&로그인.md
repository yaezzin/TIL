# [DRF] JWT를 사용한 회원가입과 로그인

## JWT

Header(헤더).PayLoad(내용).Signature(서명)




## JWT 선택 이유

* `Stateless` 서버에서 사용자 상태를 저장할 필요없이 클라이언트 측에서 사용자 정보를 유지하기 때문에 서버 부담을 줄여줌
* `모바일 환경`에서는 세션 관리 없이 인증할 수 있기 때문에 더 적합
* Built-in Token은 하나의 토큰으로 모든 세션을 관리하고, timestamp가 없어서 삭제하지 않는 이상 영구적
  * 반면 JWT는 세션 하나에 토큰 하나가 발급되고 timestamp가 존재하기 때문에 언젠간 만료
  * 로그인 시 access token, refresh token이 같이 발급되며 access token이 만료되면 refresh token을 통해 새로운 access token을 요청
  * [Django : DRF Token based Authentication VS JSON Web Token](https://stackoverflow.com/questions/31600497/django-drf-token-based-authentication-vs-json-web-token?source=post_page-----e54c3ed2420c---------------------------------------)

## 회원가입

#### 1. 필요 패키지 설치

```
$ pip install djangorestframework-simplejwt
```

#### 2. setting.py 설정

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework_simplejwt',
    ...
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
}
```

#### 3. User Model

```python
class UserManager(BaseUserManager):
    def create_user(self, phone_number, password, nickname):
        user = self.model(phone_number=phone_number, nickname=nickname)
        user.set_password(password)
        user.save(using=self.db)   
        return user
    
    def create_superuser(self, phone_number, password, nickname):
        user = self.create_user(phone_number, password, nickname)
        user.is_admin = True
        user.save(using=self.db)       
        return user

class User(AbstractBaseUser):
    phone_number = models.CharField(max_length=11, unique=True)
    nickname = models.CharField(max_length=30)
    is_active = models.BooleanField(default=True)
    is_admin = models.BooleanField(default=False)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    objects = UserManager()

    USERNAME_FIELD = 'phone_number'
    REQUIRED_FIELDS = ['nickname']

    def __str__(self):
        return self.phone_number
```

#### 4. 회원가입 뷰

```python
class RegisterView(generics.CreateAPIView):
    serializer_class = UserSerializer

    def post(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        if serializer.is_valid():
            user = serializer.save()

            token = RefreshToken.for_user(user)
            refresh_token = str(token)
            access_token = str(token.access_token)

            return Response({
                "user": serializer.data,
                "refresh": refresh_token,
                "access": access_token
            }, status=status.HTTP_201_CREATED)
        
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

#### 4. UserSerializer

```python
class UserSerializer(serializers.ModelSerializer):
    password = serializers.CharField(write_only=True)
    
    class Meta:
        model = User
        fields = '__all__'
    
    def create(self, validated_data):
        user = User.objects.create_user(
            phone_number=validated_data['phone_number'],
            nickname=validated_data['nickname'],
            password=validated_data['password']
        )
        return user
```

#### 5. urls.py

```
urlpatterns = [
    path("register/", RegisterView.as_view(), name='register'),
]
```

## 로그인
