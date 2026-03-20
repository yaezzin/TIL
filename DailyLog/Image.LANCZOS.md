## 📌 Pillow 리사이즈 필터: ANTIALIAS → LANCZOS

#### 💡 계기

PR 리뷰 중 이미지 처리 코드에서 `Image.ANTIALIAS`와 `Image.LANCZOS`가 혼용되어 있는 것을 발견
→ Pillow 10에서 `ANTIALIAS`가 제거되었다는 점을 알게 됨

#### 📚 개념 정리

* `Image.ANTIALIAS`

  * 과거 Pillow에서 사용되던 고품질 리사이즈 필터
  * 내부적으로 **LANCZOS 알고리즘과 동일**
  * Pillow 10에서 **완전히 제거됨 (deprecated → removed)**

* `Image.LANCZOS`

  * 현재 권장되는 고품질 리사이즈 필터
  * 다운샘플링 시 가장 품질이 좋은 편 (대신 속도는 조금 느림)
  * `ANTIALIAS`의 공식 대체재

#### 🔍 포인트

* `ANTIALIAS == LANCZOS (동일한 알고리즘)`
* 차이는 **이름만 다르고, 현재는 LANCZOS만 사용 가능**
* 따라서 앞으로는 **무조건 `Image.LANCZOS` 사용해야 함**


#### 🧠 배운 점

* 라이브러리 deprecated 사항을 방치하면 **버전 업 시 바로 장애로 이어질 수 있음**
* 같은 기능이라도 **코드베이스 내에서 일관성 유지가 중요**
* “지금 문제 없으니까 놔두자”보다
  → **미리 통일해두는 게 유지보수에 훨씬 유리**

