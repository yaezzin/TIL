## 프로세스 제어 블록 PCB 

> 프로세스를 실행할 때 관련된 정보를 저장하는 자료구조

* 프로세스는 한정된 자원 속에서 정해진 시간만큼 CPU를 사용하고, 시간이 끝났음을 알리는 인터럽트가 발생하면 자신의 차례를 양보하고 다음 차례가 올 때까지 기다려야 한다.
* 이처럼 운영체제는 프로세스의 실행 순서를 관리하고 프로세스에 CPU를 비롯한 자원을 배분하기 위해 PCB를 이용한다.

## 구성요소

<img width="430" alt="스크린샷 2023-06-23 오후 2 41 48" src="https://github.com/yaezzin/TIL/assets/97823928/60588144-f5c6-4149-91fb-dac077e67aee">

### 1. 프로세스 ID

* 특정 프로세스를 식별하기 위해 부여하는 고유한 번호
* 메모리에는 여러 프로세스가 있기 때문에 이를 구별하기 위한 식별자가 프로세스 ID이다.

### 2. 레지스터 값

* 프로세스가 자신의 실행 차례가 오면 **이전 작업에 대한 정보**(레지스터의 중간 값)들을 모두 복원하여 이어서 실행하게 된다.
* 따라서 PCB안에는 해당 프로세스가 실행하며 사용했던 프로그램 카운터를 비롯한 레지스터 값들이 담긴다.
* ```프로그램 카운터``` : 다음에 실행될 명령어의 위치를 가리키는 값이 프로그램 카운터입니다. 프

### 3. 프로세스 상태

* 실행, 준비, 대기, 종료 등 현재 프로세스의 상태를 PCB에 기록한다.

### 4. CPU 스케줄링 정보

* 프로세스 간에 실행 우선 순위가 다르기 때문에 프로세스가 언제, 어떤 순서로 CPU를 할당받을 것인지에 대한 정보도 PCB에 기록된다.

### 5. 메모리 관리 정보

* 프로세스마다 메모리에 저장된 위치가 다르기 때문에 PCB에는 프로세스의 ```저장 위치``` 정보를 기록한다.
* 프로세스의 주소를 알기 위해 ```페이지 테이블``` 정보도 담기게 된다.
* 또한 여러 프로세스 간 상호 메모리 보호를 위해 ```경계 레지스터 값```과 ```한계 레지스터 값```을 저장한다.

### 6. 열린 파일 목록

* 프로세스가 실행 과정에서 특정 입출력장치나 파일을 사용할 때 해당 내용이 PCB에 명시된다.

## 문맥 교환

<img width="499" alt="스크린샷 2023-06-23 오후 3 06 01" src="https://github.com/yaezzin/TIL/assets/97823928/c2d7a4b2-4933-4774-ba3c-91550fed889f">

> 기존 프로세스의 문맥을 PCB에 백업하고, 새로운 프로세스를 실행하기 위해 문맥을 PCB로 복구하는 과정 
* 프로세스가 할당된 시간을 전부 사용한 Timeout이나 I/O 요청 시스템 호출같은 상황에서 문맥 교환이 발생한다.
* ```문맥```은 프로세스 수행을 재개하기 위해 기억해야 할 정보로, 실행되던 프로세스는 지금까지의 작업 정보를 백업해놔야 다음 차례가 왔을 때 이전까지 실행했던 내용에 이어 다시 실행을 재개할 수 있기 때문에 PCB에 문맥을 기록한다.

## 참고자료
* https://yoongrammer.tistory.com/52
