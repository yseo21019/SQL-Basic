# SQL_BASIC 6주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_6th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**6주차 과제는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_6th

### 섹션 6. 다량의 자료를 연결 : JOIN 

### 5-1. Intro

### 5-2. JOIN 이해하기

### 5-3. 다양한 JOIN 방법

### 5-4. JOIN 쿼리 작성하기 

### 5-5. JOIN을 처음 공부할 때 헷갈렸던 부분

### 5-6. JOIN 연습문제 1~2번

### 5-6. JOIN 연습문제 3~5번

### 5-7. 정리



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | ✅         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<!-- 여기까진 그대로 둬 주세요-->

<br>

---

# 1️⃣ 개념정리

## 5-2. JOIN 이해하기

~~~
✅ 학습 목표 :
* JOIN에 대한 정의와 필요성에 대해 설명할 수 있다.
~~~

### 포켓몬으로 JOIN 이해하기

- pokemon 테이블: `id`, `name`, `age`, `preferred_pokemon_type`등의 칼럼

- trainer 테이블: `id`, `kor_name`, `type1`, `type2`, `total`등의 칼럼

**두 데이터를 연결하고 싶으나 연결할 수 있는 공통 값이 없음!**
 (`id`가 언뜻 봐서는 같아 보이나 사실 대상이 서로 다름)
 -> **trainer_pokemon 테이블에 이 두 데이터를 연결할 수 있는 공통값이 있다.**


이 데이터는 `trainer_id`와 `pokemon_id`를 아니 어떤 트레이너가 어떤 포켓몬을 잡았는지는 알 수 있으나, 트레이너와 포켓몬 자체의 정보는 없음 -> pokemon 데이터와 trainer 데이터를 JOIN 해서 알아보자!

- `trainer_id`: trainer 테이블의 `id`와 동일 -> `trainer_id` 컬럼 기준으로 같은 Key값을 가진 트레이너 데이터를 JOIN함(칼럼이 추가됨)
- `pokemon_id`: pokemon 테이블의 `id`와 동일 -> `trainer_id` 컬럼 기준으로 같은 Key값을 가진 포켓몬 데이터를 JOIN함(칼럼이 더 추가됨)

  -> 이처럼 여러 테이블 JOIN 가능(개수 제한 없음)

---

### SQL JOIN

- **서로 다른 데이터 테이블을 연결하는 것**
- 공통적으로 존재하는 컬럼(=Key)이 있다면, JOIN 할 수 있음
- 보통 id값을 Key로 많이 사용. 특정 범위(DATE 등)로도 JOIN 가능
- 테이블 구조에 익숙하지 않아서 어려움을 느낄 확률 높음 -> 원본 데이터가 저장된 형태를 우선 잘 파악하고, JOIN 후의 모습 예상해보며 쿼리 실행해보기!

---

### JOIN을 해야하는 이유

대부분의 회사에서는 관계형 데이터베이스 설계시 정규화 과정 거침
- 정규화는 중복을 최소화하게 데이터를 구조화하는 것
  - User Table은 유저 데이터만, Order Table은 주문 데이터만 
- 데이터를 다양한 Table에 저장해둔 뒤 필요할 때 JOIN 해서 사용
- 저장공간 효율성이나 개발 관점에서는 분리되어 있는 것이 훨씬 유리

데이터 웨어하우스에서 JOIN + 필요한 연산을 해서 **데이터 마트**를 만듦
- 데이터 웨어하우스에는 모든 데이터 전체가 보관돼있다면, 데이터 마트에는 특정 분석 목적에 맞게 데이터를 JOIN하고 가공하여 저장해두어 바로 사용할 수 있도록 한다.

<br>

## 5-3. 다양한 JOIN 방법

~~~
✅ 학습 목표 :
* JOIN 방법들의 종류를 설명할 수 있다. 
* 각 JOIN 방법들의 차이점에 대해서 설명할 수 있다. 
~~~

### INNER JOIN (A ∩ B)
**두 테이블의 공통 요소만 연결**

-> 공통 Key가 있는 행만 반환.

---

### LEFT/RIGHT (OUTER) JOIN 
**왼쪽/오른쪽 테이블 기준으로 연결**

-> 왼쪽 행을 그대로 가져오고, 오른쪽 테이블에 없는 값에 대해서는 NULL 처리.(vice versa)

---

### FULL (OUTER) JOIN (A ∪ B)
**양쪽 기준으로 연결**

-> 일치하지 않는 Key까지 모두 포함. 없는 값은 NULL 처리.

---

### CROSS JOIN (A X B)
**두 테이블의 각각의 요소를 곱함**

-> 가능한 모든 조합 반환. 


<br>

## 5-4. JOIN 쿼리 작성하기 

~~~
✅ 학습 목표 :
* JOIN을 사용한 문법에 대해 이해하여 적용할 수 있다.
* JOIN 을 활용한 쿼리를 작성할 수 있다. 
~~~

### SQL JOIN 쿼리 작성하는 흐름

1. 테이블 확인: 테이블에 저장된 데이터, 컬럼 확인

2. 기준 테이블 정의: 가장 많이 참고할 기준 테이블 정의
(기준 테이블은 row수가 적으면서, 풀려고 하는 문제에 대해 필요한 것이 다 들어있는 테이블선정)

3. JOIN Key 찾기: 여러 테이블과 연결할 Key(ON) 정리

4. 결과 예상하기: 결과 테이블을 예상해서 손, 엑셀로 작성(일종의 정답지 역할로 실제 쿼리 실행결과와 동일한지 비교해보기)

5. 쿼리 작성 / 검증: 정답지와 비교해보며 원하는 결과가 나오는지 검증

### SQL JOIN 문법

INNER JOIN
~~~sql
SELECT
 A.col1,
 A.col2,
 B.col5,
 B.col11 -- Alias(별칭) 사용 가능
FROM table_a AS A
INNER JOIN table_b AS B
ON A.Key = B.Key -- Alias(별칭) 사용 가능
~~~

LEFT/RIGHT JOIN
~~~sql
SELECT
 col
FROM table_a AS A
LEFT/RIGHT JOIN table_b AS B
ON A.Key = B.Key
~~~

FULL JOIN
~~~sql
SELECT
 col
FROM table_a AS A
FULL JOIN table_b AS B
ON A.Key = B.Key
~~~

CROSS JOIN - ON이 필요하지 않음 
~~~sql
SELECT
 col
FROM table_a AS A
CROSS JOIN table_b AS B
~~~


### 포켓몬으로 JOIN 이해하기 - 쿼리 작성

~~~sql
SELECT
 tp.*,
 t.* EXCEPT(id), -- tp에 있는 trainer_id와 중복이기 때문
 p.* EXCEPT(id) -- tp에 있는 pokemon_id와 중복이기 때문
FROM basic.trainer_pokemon AS tp
LEFT JOIN basic.trainer AS t
ON tp.trainer_id = t.id
LEFT JOIN basic.pokemon AS p -- JOIN은 계속 이어서 할 수 있음
ON tp.pokemon_id = p.id
~~~

<br>

## 5-5. JOIN 공부하면서 헷갈리는 부분

### 여러 JOIN 중 어떤 것을 사용해야 할까?
- **하려고 하는 작업의 목적에 따라 선택해보기**
- 교집합을 알고 싶으면 INNER JOIN, 가능한 모든 조합을 알고 싶으면 CROSS JOIN 등등 
- 하나를 완벽하게 체화해서 주로 사용하는 것을 추천 (LEFT JOIN 추천)

---

### 어떤 테이블을 왼쪽에 둬야할까? 

- LEFT JOIN의 경우 기준이 되는 테이블을 왼쪽에 두기
- Ex) ORDER 테이블과 User 정보 테이


## 5-6. JOIN 연습문제 1~5번 

~~~
✅ 학습 목표 :
* 연습문제(3문제 이상) 푼 것들 정리하기
~~~

### 연습문제 1. 트레이너가 보유한 포켓몬들은 얼마나 있는지 알 수 있는 쿼리를 작성하시오. 

예상결과: 
| pokemon_name  | pokemon_cnt   | 
| ----- | ------- | 
| 피카츄 | N | 
| 파이리 | N | 
| 라이츄 | N | 


~~~
# 쿼리를 작성하는 목표, 확인할 지표: 포켓몬 이름 명시, 포켓몬의 수
# 쿼리 계산 방법: trainer_pokemon(status가 active이거나 training) + pokemon JOIN -> GROUP BY 후 COUNT
# 데이터의 기간: X
# 사용할 테이블: trainer_pokemon, pokemon
# Join KEY: trainer_pokemon.pokemon_id = pokemon.id
# 데이터 특징: '보유했다'는 status가 Active, Training임을 의미. Released는 방출했다는 것을 의미.
~~~

~~~
1) trainer_pokemon에서 status가 Active이거나 Training인 경우를 필터링(WHERE)
 -- 연산량 관점에서 row를 줄이고 JOIN하는 것이 더 효율적.
 -- 핵심! 항상 JOIN 하기 전에 Table을 그대로 사용해야 하는지, 혹은 줄이고 쓰는게 내 목적에 더 맞는지 확인하기!
2) 필터링 결과를 trainer 테이블과 JOIN
3) 2)의 결과에서 pokemon_name, COUNT(pokemon_id) AS pokemon_cnt를 SELECT
~~~
 
**답안 쿼리**
~~~sql
SELECT
 kor_name, -- 중복되는 칼럼명이 없으면 테이블 안 밝혀도 되나 id는 중복되기에 tp.id라고 밝혀야함.
 COUNT(tp.id) AS pokemon_cnt
FROM(
  SELECT
  id,
  trainer_id,
  pokemon_id,
  status
  FROM basic.trainer_pokemon
  WHERE 
  status IN ("Active", "Training")
) AS tp
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id  = p.id
GROUP BY
 kor_name
ORDER BY 
 pokemon_cnt DESC
~~~

**참고) 1=1**
- `1 = 1` 은 항상 참인 기본 조건을 만들어 놓고, 뒤에 조건을 자유롭게 붙이기 위한 SQL 패턴
~~~sql
WHERE
 1 = 1
 AND status = "Active"
 AND status = "Training"
~~~
- 쿼리 전체 결과에 영향 없이 여러 조건을 빠르게 주석처리하거나 추가할 수 있음
### 연습문제 2. 각 트레이너가 보유한 포켓몬 중에서 'Grass' 타입의 포켓몬 수를 계산하시오. (Type1 기준으로)

~~~
# 쿼리를 작성하는 목표, 확인할 지표: 트레이너가 보유한 포켓몬 중 GRASS 타입
# 쿼리 계산 방법: trainer_pokemon(status가 active이거나 training) + pokemon JOIN -> GRASS 타입으로 WHERE 조건 건 후 COUNT
# 데이터의 기간: X
# 사용할 테이블: trainer_pokemon, pokemon
# Join KEY: trainer_pokemon.pokemon_id = pokemon.id
# 데이터 특징: '보유했다'는 status가 Active, Training임을 의미. Released는 방출했다는 것을 의미.
~~~

~~~
# 어떤 테이블을 왼쪽에 둬야할까
 - trainer_pokemon 테이블: 트레이너가 잡은 포켓몬 저장됨. 트레이너가 보유했던/보유한 포켓몬 알려줌
 - pokemon 테이블: 포켓몬의 메타 정보
 - trainer_pokemon을 왼쪽에 두면 -> 트레이너가 보유한/보유했던 데이터를 기반으로 포켓몬 데이터만 추가. NULL 추가 X
 - pokemon을 왼쪽에 두면 -> 포켓몬 중 보유되지 않은 것들은 trainer_pokemon에 없으니 NULL이 추가됨.
 - 보통은 가능한 JOIN Key가 많고 내가 구하고자 하는 데이터가 비슷한 테이블일수록 왼쪽에 둠.
~~~


**답안 쿼리**
~~~sql
SELECT
 p.type1,
 COUNT(tp.id) AS grass_cnt
FROM(
  SELECT
  id,
  trainer_id,
  pokemon_id,
  status
  FROM basic.trainer_pokemon
  WHERE 
  status IN ("Active", "Training")
) AS tp
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id  = p.id
WHERE 
 type1 = "Grass"
GROUP BY type1
ORDER BY 
 2 DESC -- 2 대신에 grass_cnt도 가능
~~~

### 연습문제 3. 각 트레이너의 hometown과 포켓몬을 포획한 location을 비교하여, 자신의 고향에서 포켓몬을 포획한 트레이너의 수를 계산하시오. (Status 상관없이)

~~~
# 쿼리를 작성하는 목표, 확인할 지표: 트레이너의 고향과 포켓몬 포획 위치가 같은 트레이너의 수 계산
# 쿼리 계산 방법: trainer.hometown, trainer_pokemon.location을 JOIN -> hometown = location 조건 -> 트레이너의 수 COUNT
# 데이터의 기간: X
# 사용할 테이블: trainer, trainer_pokemon
# Join KEY: trainer_pokemon.trainer_id = trainer.id
# 데이터 특징

~~~
**답안 쿼리**
~~~sql
SELECT
  COUNT(DISTINCT tp.trainer_id) AS trainer_uniq,  
  COUNT(tp.trainer_id) AS trainer_cnt            
FROM basic.trainer AS t
LEFT JOIN basic.trainer_pokemon AS tp
  ON t.id = tp.trainer_id
WHERE
  tp.location IS NOT NULL
  AND t.hometown = tp.location
~~~

<br>

<br>

![alt text](<스크린샷 2025-11-10 233941.png>)
---

# 2️⃣ 확인문제 & 문제 인증


## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/164673

> 조건에 부합하는 중고거래 댓글 조회하기 (JOIN)

https://school.programmers.co.kr/learn/courses/30/lessons/144854

> 조건에 맞는 도서와 저자 리스트 출력하기 (JOIN)

![alt text](<스크린샷 2025-11-10 234511.png>)
![alt text](<스크린샷 2025-11-10 234620.png>)


---

# 3️⃣ 참고자료

JOIN 에 대해서 그림으로 쉽게 이해할 수 있는 자료들도 있어서 첨부합니다. 아래의 블로그도 학습할 때 같이 참고해주세요.

1. https://data-marketing-bk.tistory.com/entry/SQL-JOIN-%ED%95%9C-%EB%B0%A9%EC%97%90-%EC%A0%95%EB%A6%AC-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%BD%94%EB%93%9C%EA%B9%8C%EC%A7%80-%EC%9D%B4%EA%B2%83%EB%A7%8C-%EB%B3%B4%EC%9E%90



2. https://velog.io/@wijoonwu/JOIN

<br>

### 🎉 수고하셨습니다.
