# stash
<img width="408" alt="스크린샷 2025-04-20 오후 10 27 41" src="https://github.com/user-attachments/assets/693cf3eb-4c73-48ce-a220-3952fe787428" />

## git stash란?

아직 마무리하지 않은 작업을 스택에 잠시 저장할 수 있도록하는 명령어  
* `branch1`에서 작업을 하던 중 `branch2`로 변경해야 할 일이 있다고 가정한다면.. 아직 완료하지 않은 일을 커밋하기 보다 stash를 이용하자

## 적용 범위

**1️⃣ 작업 디렉토리의 수정 내용 (Modified)**

**2️⃣ staging aread에 있는 파일들**
  * git add 명령어를 실행한 파일들

## 명령어

```
git stash
git stash list               # stash 목록 확인
git stash apply              # stash 적용
git stash apply [stash 이름]
git stash drop               # 스택에 있는 stash 제거
git stash pop                # 적용 + 스택에서 제거
```
```
git stash apply          # 이전에 git add .한 내용을 staging 상태로 복원하지 않음
git stash apply --index  # 복원 + git add 상태 그대로!
```

## 충동 시 stash 사용법

```
git checkout branch1

# 파일 내용 수정
echo "branch1에서 수정한 내용" > test.txt

# git add 안 한 상태라고 가정
git stash

# 브랜치 이동
git checkout branch2

# 같은 파일을 수정
echo "branch2에서 수정한 내용" > test.txt
git add .
git commit -m "branch2에서 test.txt 수정"

# 다시 branch1으로 돌아가 stash 적용 -> 충돌발생
git checkout branch1
git stash apply
```


```
<<<<<<< Updated upstream
branch2에서 수정한 내용
=======
branch1에서 수정한 내용
>>>>>>> Stashed changes
```
* test.txt 내부는 이러할 것,,
  
```
# 원하는 내용으로 수정 후 다음 명령어 수행
git add test.txt
git commit -m "충돌 해결"
```


