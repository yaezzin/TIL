# [Pycon] Django ORM(QuerySet) 구조와 원리 그리고 최적화전략

🌱 [Pycon 보고 정리하기](https://www.youtube.com/watch?v=EZgLfDrUlrk)

## 1. Lazy Loading 지연로딩 : 정말 필요한 시점에 SQL을 호출한다.

```python
users: QuerySet = User.objects.all() # User를 선언하는 시점에 users는 쿼리셋임 (리스트가 아님, select 쿼리가 날라가지 않음)
user_list: List[User] = list(users)  # 리스트로 묶는 시점에 실제 SQL이 호출됨
```
* 쿼리셋을 사용하는 시점에 SQL이 날라간다

```python
users: QuerySet = User.objects.all()
first_user = users[0]
user_list: List[User] = list(users)
```
* users[0] 코드에서 유저 한명만 얻기 위해서 `select * from User limit 1;`을 호출할 것임
* list(users) 코드에서 모든 유저 목록을 얻기 위해 `select * from User;`를 호출할 것임
* 결론 : 불필요하게 SQL이 두번 호출됨 -> SQL을 한번만 호출해서 데이터를 재사용하면 되지만 ORM은 이후 로직을 알지 못하므로,,

```python
users: QuerySet = User.objects.all()
user_list: List[User] = list(users)
first_user = users[0]
```
* 쿼리셋을 호출하는 순서를 바꿔준다면?
* user_list에 모든 유저를 가져오는 SQL이 호출되어 모든 유저 데이터가 캐싱되어 있을 것임
* 따라서 `select * from User;`한번만 호출할 수 있게 됨


```python
users: QuerySet = User.objects.all() # 유저를 선언하는 시점에는 SQL이 호출되지 않음

for user in users: # 여기서 모든 유저를 조회하는 SQL 1번 호출되고
    user.userinfo  # user.userinfo를 조회할 때마다 SQL이 호출됨 
```
* N + 1 문제 발생 -> django는 select_related(), prefetch_related()라는 메서드 를 제공함

## 2. QuerySet

<img width="1045" alt="스크린샷 2025-04-21 오후 10 59 50" src="https://github.com/user-attachments/assets/1c7c9fa7-5e05-4722-99d4-60446ba8150a" />


## 3. Testcase [CaptureQueriesContext]


## 4. prefetch_related()와 filter()는 완전 별개 문제다.

<img width="700" alt="스크린샷 2025-04-21 오후 11 20 15" src="https://github.com/user-attachments/assets/24e0b386-e5b2-4b3e-becb-5736615847ee" />

```python
company_queryset = (
          Company.objects
                  .prefetch_related("product_set")
                  .filter(name="company_name1", product_name__isnull=Flase)
)
```
* 이러한 경우 필터가 먼저 작동함 -> 필터가 작동하기 위해서는 product_name이 필요 -> 이너 조인으로 product 테이블과 조인한 이후에 isnull 조건을 적용하게 될 것임
* 이후에 prefetch_related를 위해 추가 쿼리가 한번 더 동작함

#### 방법 1

```python
Company.objects
       .filter(name="company_name1", product__name__isnull=False)
```

#### 방법 2
```python
Company.objects
       .filter(name="company_name1") # company에 대한건 필터 걸고
       .prefetch_related( # product에 대한 필터는 Prefetch에 제공하자
           "product_set", Prefetch(queryset=Product.objects.filter(product_name__isnull=False))
       )
```

#### 결론
* prefetch_related()를 filter() 뒤에 두어서 실수를 방지하자
* `annotate` - `select_related` - `filter` - `prefetch_related` 순으로 작성하는 것을 추천 (실제 발생하는 SQL의 순서와 가장 유사하므로) 
