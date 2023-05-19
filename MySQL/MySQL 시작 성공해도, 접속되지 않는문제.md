# MySQL을 시작 성공해도, 접속되지 않는 문제

## 문제 상황
<img width="840" alt="스크린샷 2023-05-19 오후 6 30 23" src="https://github.com/yaezzin/TIL/assets/97823928/7f170c03-3a74-44fb-9e7c-a584faca3d98">

---
mysql에 접속하기 위해 ```brew services restart mysql``` 명령어를 통해 재시작을 했고,   
==> Successfully started `mysql` (label: homebrew.mxcl.mysql)라는 성공 문구를 확인헀습니다.  
하지만 ```brew services list``` 명령어를 실행해보면 mysql은 여전히 stopped라고 되어있었습니다. 

## 해결방법

#### 1. mysql 중단

```
brew services stop mysql
```
#### 2. 삭제

```
rm ~/Library/LaunchAgents/homebrew.mxcl.mysql@5.7.plist
```
* MySQL 관련된 파일을 정리하기 위해 homebrew.mxcl.mysql.plist 파일을 삭제합니다.
* brew services list를 입력해보면 mysql 가장 맨 오른쪽에 ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist가 보이는데 이것을 삭제해준다.


```
brew unlink mysql
brew uninstall mysql
rm -rf /opt/homebrew/var/mysql
```
* MySQL 데이터 디렉토리를 삭제합니다.

```
cp /opt/homebrew/etc/my.cnf /opt/homebrew/etc/my.cnf.backup
rm /opt/homebrew/etc/my.cnf

```
* 기존의 my.cnf 파일을 백업하고 삭제합니다.

```
brew install mysql
brew link mysql
```
* mysql을 재설치합니다.

```
vi .~zshrc

# 그안에 입력
export PATH="/opt/homebrew/opt/mysql/bin:$PATH"
```

<img width="763" alt="스크린샷 2023-05-19 오후 6 34 57" src="https://github.com/yaezzin/TIL/assets/97823928/7805f389-4a1b-4628-8eff-eefaa8131e4f">

이제 MySQL을 시작하고 mysql -u <사용자명> -p 명령을 실행하여 MySQL에 정상적으로 접속할 수 있었습니다

## 참고
* https://stackoverflow.com/questions/70447598/brew-start-and-brew-restart-wont-start-service




