## Shell

운영체제상에서 사용자가 입력하는 명령을 읽고 해석하여 대신 실행해주는 프로그램

## Shell Script

쉘에서 실행되는 스크립트 파일
* 일련의 명령어를 한 번에 실행 → 반복적인 일을 자동화


### 실습

```
$ touch pip_install.sh
```

* ```touch``` : 파일의 날짜와 시간을 수정하는 명령어이긴 하지만, 0바이트 파일을 생성하기 위해 자주 사용

```
#!/bin/bash

sudo apt install python3-pip
```

* ```#!```를 통해서 Script파일의 최상단에 해당 파일을 해석해줄 인터프리터의 절대경로를 지정 


```
$ chmod +x pip_install.sh
```
* ```chmod +x``` : x 권한 부여
* ```chmod u+x``` : 소유주에게만 x 권한 부여





