## APIView

```python
class UserAPIView(APIView):
    def get(self, request):
        users = User.objects.all()
        serializer = UserSerializer(users, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = UserSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

```python
# urls.py

urlpatterns = [
  path('users/', UserAPIView.as_view()),
]
```

* 일반적인 API뷰로, HTTP 메서드별(`get()`, `post()` 등)로 직접 구현하면, 해당 메서드 요청이 들어올 때 호출됨
* 하나의 CBV이므로 **하나의 URL만 처리 가능**


## Mixins 상속

```python

class UserListMixins(mixins.ListModelMixin, mixins.CreateModelMixin,generics.GenericAPIView):
    queryset = User.objects.all()
    serializer_classs = UserSerializer

    def get(self, , request, *args, **kwargs):
        return self.list(request)

    def post(self, , request, *args, **kwargs):
        return self.create(request)
```

```python
urlpatterns = [
    path('mixin/post/', views.UserListMixins.as_view()),
]
```

* APIView는 각 요청 메서드마다 직접 serializer 처리를 해주기 때문에 중복 발생
* queryset과 serializer_class를 지정해주면 나머지는 상속받은 Mixin과 연결해주면 됨

## generics APIView

```python
# views.py

class UserListGenericAPIView(generics.ListCreateAPTIView):
    queryset = User.objects().all()
    serializer_class = UserSerializer

class UserDetailGenericAPIView(generics.RetrieveUpdateDestroyAPIView):
    queryset = User.objects().all()
    serializer_class = UserSerializer    
```
* Mixin을 상속함으로써 반복되는 내용을 많이 줄일 수 있으나, 여러 개를 상속하게 되면서 가독성이 떨어짐. 따라서 rest_framework에는 저들을 상속한 새로운 9개의 클래스를 정의해둠

```
generics.CreateAPIView : 생성
generics.ListAPIView : 목록
generics.RetrieveAPIView : 조회
generics.DestroyAPIView : 삭제
generics.UpdateAPIView : 수정
generics.RetrieveUpdateAPIView : 조회/수정
generics.RetrieveDestroyAPIView : 조회/삭제
generics.ListCreateAPIView : 목록/생성
generics.RetrieveUpdateDestroyAPIView : 조회/수정/삭제
```


## ViewSet

```python
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

```python
# urls.py
router = DefaultRouter()
router.register(r'users', UserViewSet)

urlpatterns = [
  path('', include(router.urls)),
]
```
* generics APIView를 통해서 코드를 간소화하여도, 여전히 공통된 부분인 queryset, serializer_class가 존재하는데, 이를 ViewSet이 한번에 처리해준다.
* ViewSet은 CBV가 아닌 헬퍼 클래스로 2종류가 있음
  * `viewsets.ReadOnlyModelViewSet` : 목록 조회, 특정 레코드 조회
  * `viewsets.ModelViewSet` : 목록 조회, 특정 레코드 생성/조회/수정/삭제
* `list()`, `retrieve()`, `create()`, `update()` 같은 CRUD 기능을 자동으로 제공


