# SQL_BASIC 3주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_3rd_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**3주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **7 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 3문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_3rd

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-6. 연습문제 1~3번

### 2-6. 연습문제 7~9번

### 2-6. 연습문제 10~12번

### 2-6. 연습문제 13~17번

### 2-7. 정리 

### 2-8. 새로운 집계함수



## 섹션 4. 쿼리 잘 작성하기, 쿼리 작성 템플릿 및 오류를 잘 디버깅하기

### 3-1. INTRO

### 3-2. SQL 쿼리 작성하는 흐름

### 3-3. 쿼리 작성 템플릿과 생산성 도구 



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | 🍽️         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 2-6. 연습문제

~~~
✅ 학습 목표 :
* 연습문제(7문제 이상) 푼 것들 정리하기
~~~

### 연습문제 1. 포켓몬 중에 type2가 없는 포켓몬의 수를 알려주는 쿼리를 작성하시오.
`NULL`
 - 0도 아니고, " "도 아님 -> '값이 없는 상태'를 의미
 - 연산자: `IS NULL` (NULL이 아닌 값은 `IS NOT NULL`로 작성)
 - NULL은 다른 값과 직접 비교할 수 없기에 '=' 사용할 수 없음. NULL = NULL은 거짓도 참도 아닌 알 수 없음

답:
~~~sql
SELECT
 COUNT(id) AS cnt
FROM basic.pokemon
WHERE 
 type2 IS NULL   
~~~

`AND/OR`
- `WHERE` 절에서 여러 조건을 연결(모든 조건 만족): `AND` 조건을 사용
- `WHERE` 절에서 여러 조건을 연결(여러개 중 하나만 만족): `OR` 조건을 사용
- EX)
~~~sql
SELECT
 *
FROM basic.pokemon
WHERE 
 type2 IS NULL
 AND type1 = "Fire"

SELECT
 *
FROM basic.pokemon
WHERE 
 (type2 IS NULL)
 OR (type1 = "Fire")     --OR 조건끼리는 ()로 구분한다.
~~~

### 연습문제 2. type2가 없는 포켓몬의 type1과 type1의 포켓몬의 수를 알려주는 쿼리를 작성하시오. 단, type1의 포켓몬 수가 큰 순으로 작성하시오.
답:
~~~sql
SELECT
 type1,
 COUNT(type1) AS cnt   --여기서 COUNT(*)은 type1이 NULL인 값도 포함하나 
FROM basic.pokemon     --COUNT(type1)은 NULL인 값은 제거하고 집계하는 것이 차이점 
WHERE 
 type2 IS NULL
GROUP BY type1
ORDER BY cnt DESC
~~~
- 주의: 집계함수는 무조건 `GROUP BY`와 같이 쓰임. 만약 집계하고자 하는것이 테이블 전체를 대상이면 안써도 상관없으나 그룹별 집계값을 구하고싶으면 꼭 필요함.

### 연습문제 3. (type2 상관없이) type1의 포켓몬 수를 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 type1,
 COUNT(id) AS cnt
 --(=)COUNT(DISTINCT id) AS cnt
FROM basic.pokemon
GROUP BY type1
~~~
`DISTINCT`
- Unique한 값을 알고 싶을 때 사용. 
- id같은 컬럼은 일반적으로 중복이 없기에 `DISTINCT`를 쓴것과 안쓴것 차이가 없으나, 중복이 있게 설계된 컬럼들은 `DISTINCT`를 이용해서 파악해야 할 수도 있음(DAU 등).

### 연습문제 4. 전설 여부에 따른 포켓몬 수를 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 is_legendary,    --컬럼 이름의 앞부분을 입력하고 기다리면 자동 완성 할 수 있음!
 COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY is_legendary
~~~
`GROUP BY`/`ORDER BY` 뒤에 굳이 컬럼명을 쓰지 않고, 숫자로 대체 가능: 이 숫자는 `SELECT` 뒤에 붙는 컬럼의 순서에 따라 정해짐.

### 연습문제 5. 동명 이름을 찾아내는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 name,
 COUNT(name) AS trainer_cnt
FROM basic.trainer
GROUP BY name
HAVING cnt >= 2
~~~
답2(서브쿼리):
~~~sql
SELECT
 *
FROM(
 SELECT
  name,
  COUNT(name) AS trainer_cnt
 FROM basic.trainer
 GROUP BY name
)
WHERE trainer_cnt >= 2
~~~

### 연습문제 6. trainer 테이블에서 "Iris" 트레이너의 정보를 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 *
FROM basic.trainer
WHERE 
 name = "Iris"
~~~

### 연습문제 7. trainer 테이블에서 "Iris", "Whitney", "Cynthia" 트레이너의 정보를 알 수 있는 쿼리를 작성하시오.
답: 
~~~sql
SELECT
 *
FROM basic.trainer
WHERE 
--  (name = "Iris")
--  OR (name = "Whitney")
--  OR (name = "Cynthia")      -- OR 조건으로 쓰기 귀찮다->IN 사용 가능
 name IN ("Iris", "Whitney", "Cynthia") --name에 괄호 안에 value가 있는 row만 추출
~~~

### 연습문제 8. 전체 포켓몬 수를 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 COUNT(id) AS cnt
FROM basic.pokemon
~~~

### 연습문제 9. 세대별로 포켓몬 수는 얼마나 되는지 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 generation,
 COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY generation
~~~

### 연습문제 10. type2가 존재하는 포켓몬 수는 얼마나 되는지 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 COUNT(id) AS cnt
FROM basic.pokemon
WHERE 
 type2 IS NOT NULL
~~~

### 연습문제 11. type2가 존재하는 포켓몬 중에 가장 많은 type1이 무엇인지 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 type1,
 COUNT(id) AS cnt
FROM basic.pokemon
WHERE 
 type2 IS NOT NULL
GROUP BY 1      -- 여기서 1은 type1을 의미함
ORDER BY cnt DESC
LIMIT 1         -- 마지막 출력값을 1row로 제한->최댓값만 볼 수 있음
~~~

### 연습문제 12. type이 하나만 존재하는 포켓몬 중에 가장 많은 type1이 무엇인지 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 type1,
 COUNT(id) AS cnt
FROM basic.pokemon
WHERE 
 type2 IS NULL
GROUP BY 1
ORDER BY cnt DESC
LIMIT 1
~~~

### 연습문제 13. 포켓몬의 이름에 "파"가 들어가는 포켓몬을 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT 
 kor_name
FROM basic.pokemon
WHERE 
 kor_name LIKE "%파%"
~~~
`LIKE`: 한 단어에 특정 단어가 포함되는지 알아내는 방법
 - `칼럼 LIKE "%특정단어%"`
 - `"%파"`: "파"로 끝나는 단어
 - `"파%"`: "파"로 시작하는 단어
 - `"%파%"`: "파"가 들어가는 단어 

### 연습문제 14. 뱃지가 6개 이상 있는 트레이너가 몇 명 있는지 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT 
 COUNT(id) AS cnt
FROM basic.trainer
WHERE 
 badge_count >= 6
~~~

 ### 연습문제 15. 보유한 포켓몬이 가장 많은 trainer을 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT 
 trainer_id, 
 COUNT (pokemon_id) AS cnt
 --COUNT (DISTINCT pokemon_id) AS cnt2 
 -- 한 trainer이 같은 포켓몬을 여러마리 보유할 수도 있음-> DISTINCT를 쓰면 안됨.
FROM basic.trainer_pokemon
GROUP BY trainer_id
ORDER BY cnt DESC
~~~

### 연습문제 16. 포켓몬을 가장 많이 풀어준 trainer을 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT 
 trainer_id, 
 COUNT (pokemon_id) AS cnt
FROM basic.trainer_pokemon
WHERE 
 status = "Released"
GROUP BY trainer_id
ORDER BY cnt DESC
LIMIT 1
~~~

### 연습문제 17. trainer별로 풀어준 포켓몬의 비율이 20%가 넘는 trainer을 알 수 있는 쿼리를 작성하시오.
답:
~~~sql
SELECT
 trainer_id,
 COUNTIF(status = "Released") AS released_cnt,
 COUNT(pokemon_id) AS pokemon_cnt,
 COUNTIF(status = "Released")/COUNT(pokemon_id) AS released_ratio
FROM basic.trainer_pokemon
GROUP BY
 trainer_id
HAVING 
 released_ratio >= 0.2
~~~
`COUNTIF`
- 조건을 걸고 count하는 집계함수
## 2-8. 새로운 집계함수


~~~
✅ 학습 목표 :
* SQL 쿼리 구조를 이해할 수 있다. 
* SELECT, FROM, WHERE을 활용하는 방법을 설명할 수 있다. 
~~~

### `GROUP BY ALL`
Can group rows by inferring grouping keys from the `SELECT` items-> `GROUP BY`에서 모든 컬럼 명시할 필요 없어짐.


## 3-2. 쿼리를 작성하는 흐름

~~~
✅ 학습 목표 :
* 쿼리를 작성하는 흐름을 설명할 수 있다.
~~~

**1. 지표 고민(문제정의)**: 어떤 문제를 해결하기 위해 데이터가 필요한가? 

**2. 지표 구체화**: 추상적이지 않고 구체적으로 지표 명시 -> 분자&분모 명시, 이름 구체적으로 작성.

**3. 지표 탐색**: (회사에서) 유사한 문제를 해결한 케이스가 있는지 확인. 존재하는 경우엔 해당 쿼리에서 어떠한 지표를 사용했는지, 이를 추출하기 위해 어떤 쿼리를 작성했는지 확인.

 **4. 쿼리 작성**: 데이터가 있는 테이블 찾기
 - 데이터 테이블이 2개 이상이면 연결할 수 있는 방법 고민해야함(JOIN)

**5. 데이터 정합성 확인**: 예상한 결과와 동일한지 확인

**6. 쿼리 가독성**: 나중을 위해 깔끔하게 쿼리 작성

**7. 쿼리 저장**: 쿼리는 재사용되므로 문서로 저장

`4.쿼리 작성`은 시간이 지날수록 저절로 느는 기술. 실무에서는 문제 정의하고 지표 정하는 과정이 매우 중요!


## 3-3. 쿼리 작성 템플릿과 생산성 도구

~~~
✅ 학습 목표 :
* 생산성 도구를 만들 수 있다.
~~~

**쿼리 작성 템플릿**
>#쿼리를 작성하는 목표, 확인할 지표:
>
>#쿼리 계산 방법(집계인지, 칼럼 표출인지,...):
> 
>#데이터의 기간:
>
>#사용할 테이블:
>
>#JOIN KEY:
>
>#데이터 특징:

템플릿을 사용하는 것을 까먹음 -> 생산성 도구 사용!

**생산성 도구 Espanso**



<br>
<br>

---

# 2️⃣ 학습 인증란


![alt text](KakaoTalk_20250922_230356284.jpg)


<br><br>



---

# 3️⃣ 확인문제

## 문제 1

> **🧚Q. 포켓몬 게임에 재미를 느낀 동혁은 포켓몬 도감에서 강력한 포켓몬 타입을 미리 선점하기 위해, 먼저 어떤 포켓몬들이 있는지 포켓몬 수를 기준으로 내림차순 정렬하여  확인하고자 했습니다.**
>
> 그래서 다음과 같은 필요한 정보를 미리 정리해보았습니다. 

~~~
조건 : type2는 상관없이
보고 싶은 컬럼 : type1
집계 내용 : 각 type1 별 포켓몬 수
정렬 기준 : 포켓몬 수를 기준으로 내림차순 정렬
~~~

> **이 목표를 바탕으로 동혁이 아래와 같은 쿼리를 잘 작성했지만, 일부 SQL 문법 요소를 빼먹었습니다. 비어 있는 부분인 ㄱ,ㄴ,ㄷ 에 들어갈 알맞은 SQL 구문을 채워보세요:**

~~~sql
SELECT type1, (ㄱ)
FROM pokemon
(ㄴ) type1
ORDER BY (ㄱ) (ㄷ);
~~~



~~~
ㄱ: COUNT (id) AS cnt
ㄴ: GROUP BY
ㄷ: ASC
~~~



### 🎉 수고하셨습니다.
