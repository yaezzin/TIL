## Qclass가 갑자기 인식되지 않는 문제

### 문제 상황

프로젝트를 하면서 잘되던 QueryDSLdl.. 갑자기 엔티티에 대해 Qclass를 인식하지 못하는 상황이 됨

<img width="480" alt="스크린샷 2024-04-01 오후 4 59 28" src="https://github.com/yaezzin/TIL/assets/97823928/f6ed2178-9749-4bc6-ab5d-943856ad1d2e">

* 원래는 위처럼 정상적으로 떠야하는 import문이 다 빨간줄이 떴다

### 해결

* 이는 IDE가 generated폴더를 source로 인식하지 못하기 때문에 발생하는 오류!!
* src/main/generated 폴더를 우클릭 후 mark directory as를 통해 source root로 설정했다

