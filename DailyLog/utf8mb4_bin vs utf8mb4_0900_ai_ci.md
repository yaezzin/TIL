## MySQL Collation 불일치: utf8mb4_bin vs utf8mb4_0900_ai_ci     

#### 💡 계기                                                                                                                                                               

* CONCAT(group.code, member.code, '_', CAST(id AS CHAR)) 결과와 사용자 입력값을 비교하는 쿼리를 작성.          
* `OperationalError (1267, "Illegal mix of collations (utf8mb4_bin,NONE) and (utf8mb4_0900_ai_ci,COERCIBLE) for operation '='")` 발생
  * → collation이 뭔데 비교조차 안된다는 거지?"라는 의문에서 출발                                                                                                                                    

-----
#### 📚 개념 정리

1. `Charset vs Collation`
- Charset: 문자를 어떤 바이트로 저장할지 (예: utf8mb4 = UTF-8 1~4바이트)                                                                                              
- Collation: 그 문자들을 어떻게 비교/정렬할지 (대소문자 구분? 악센트 구분?)
  - 같은 charset이라도 collation이 다르면 'A' = 'a' 의 결과가 달라짐                                                                                                    
                                                                                                                                                                      
2. `utf8mb4_bin`                                                                                                                                                                                                                                                                                                                              
- 바이트 단위 1:1 바이너리 비교         
- 대소문자 / 악센트 모두 구분 → 'A' ≠ 'a', 'é' ≠ 'e'
- 가장 엄격하고 빠름. 코드/식별자 컬럼에 자주 쓰임                                                                                                                    
                                                                                                                                                                      
3. `utf8mb4_0900_ai_ci`                                                                                                                                                                                                                                                                                                                       
- **MySQL 8 디폴트 collation**              
- 0900 = Unicode 9.0 규칙, ai = Accent Insensitive, ci = Case Insensitive
- 'A' = 'a', 'é' = 'e'. 사람이 읽는 텍스트 비교에 자연스러움                                                                                                          
                                                                                                                                                                      
4. `Coercibility (강제 변환 우선순위)`                                                                                                                                                                                                                                                                                            
* 문자열 비교 시 양쪽 collation이 다르면 MySQL은 더 약한 쪽을 강한 쪽으로 맞춤.   
* COLLATE 명시 > 컬럼 값 > 리터럴/파라미터 (COERCIBLE) > NULL
          
5. `NONE collation`
* CONCAT처럼 여러 인자를 합치는 함수에서 각 인자의 collation이 서로 호환되지 않으면 결과는 NONE. NONE은 어떤 collation으로도 강제 변환이 안 되어 비교 자체가 불가 → 1267 에러.

-----                                                                                                                                                                      
#### 🔍 포인트                               

- 한 컬럼은 utf8mb4_bin, 한 컬럼은 utf8mb4_0900_ai_ci → CONCAT 결과 = NONE                                                                                            
- 비교 우변(파이썬에서 넘긴 문자열) = COERCIBLE
- NONE = COERCIBLE → MySQL이 비교 규칙을 정할 수 없어 거부                                                                                                            
- Django 5.2 ORM에는 Collate 표현식이 없어 COLLATE를 ORM 레벨에서 명시하기 까다로움                                                                                   
- 해결책 3가지: (1) SQL COLLATE 명시 (Func 서브클래싱) / (2) 컬럼 collation 통일 (마이그레이션) / (3) SQL 비교 자체를 회피 — id로 1차 매칭 후 Python에서 prefix 비교  
                                                                                                                                                                      
#### 🧠 배운 점                                                                                                                                                            
                                                                                                                                                                      
- "같은 utf8mb4니까 비교되겠지"는 착각 — collation이 비교의 본질이고 charset은 저장 형식일 뿐                                                                         
- 컬럼 정의의 collation은 한 번 굳어지면 후속 쿼리 작성에 계속 영향을 줌 → 스키마 만들 때부터 의도적으로 통일하는 게 안전
                                                                                                         
