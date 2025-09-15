# SQL_BASIC 2주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_2nd_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**2주차 과제**는 1주차 과제처럼 SQL의 필요성이나 느낀점 위주가 아닌, **실제 강의 내용을 바탕으로 개념을 정리하고 학습한 내용을 집중적으로 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요. 

**👀(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_2nd

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-3. 데이터 탐색 (SELECT, FROM, WHERE)

### 2-4. SELECT 연습문제

### 2-5. 집계 (Group By + Having + Sum/Count)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | 🍽️         |
| 4주차 | 섹션 **3-4** ~ **4-4** | 🍽️         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리 

## 2-3. 데이터 탐색 (SELECT, FROM, WHERE)

~~~
✅ 학습 목표 :
* SQL 쿼리 구조를 이해할 수 있다. 
* SELECT, FROM, WHERE의 핵심 문법을 설명할 수 있다. 
~~~

* 포켓몬을 선택할 때 포켓몬의 이름, 공격력/방어력, 타입 등 다양한 기준으로 포켓몬을 선정함. 
  - 난 꼬부기랑 가고싶어!
  - 난 공격력이 가장 강한 포켓몬이랑 가고싶어!
  - 난 불 타입 포켓몬이랑 가고싶어!
  - 이 과정들은 쿼리를 작성하는 과정과 매우 유사함

### 쿼리문 
~~~
SELECT                #테이블의 어떤 컬럼을 선택(출력)할 것인가?
 Col1 AS new_name,    #Col1의 내용을 'new_name'으로 변경
 Col2,
 Col3
FROM Dataset.Table    #어떤 테이블에서 데이터를 확인할 것인가?
WHERE                 #원하는 조건이 있다면 어떤 조건인가?
 Col1 = 1 #조건문: Col1이 값 '1'을 갖는것만 
~~~ 

~~~
SELECT
 * 
~~~
* 모든 컬럼을 출력하겠다-> 로우데이터가 매우 많으면 비용 많이 들어서 비추-> 데이터 확인이 목적이라면 미리보기로 가능 
~~~
SELECT
 * EXCEPT(제외할 컬럼) 
~~~
* 몇개를 제외한 컬럼 출력
### FROM
 * 데이터를 확인할 Table 명시
   - 이름이 너무 길다면 AS new_name 으로 별칭 지정 가능 
   - ex) FROM Table1 AS t1
### WHERE 
 * FROM에 명시된 Table에 저장된 데이터를 필터링(조건 설정)
### SELECT 
* Table에 저장되어 있는 컬럼 선택
* 여러 컬럼 명시 가능
 * col1 AS new_name 으로 컬럼의 이름도 별칭 지정 가능 


하이라이트한 후 run하면 지정된 부분만 실행 가능

SFE 구문 뒤에 ; 쓰면 같은 쿼리 내에 새로운 SFE 구문 입력 가능


## 2-5. 집계 (Group By / HAVING / SUM,COUNT)

~~~
✅ 학습 목표 :
* 데이터를 집계하고 그룹화하는 방법을 설명할 수 있다.
* GROUP BY, HAVING, ORDER BY, 집계함수(SUM/COUNT 등)을 활용하는 방법을 설명할 수 있다.
* having과 where의 차이에 대해서 설명할 수 있다.
~~~

### 집계 GROUP BY
* 그룹화해서 계산(+, -, max, min, mean, count)하는것
* 특정 컬럼을 기준으로 모으면서 다른 컬럼에선 집계 가능
  - ex) 포켓몬의 '타입'을 기준으로 그룹화하면 타입별로 공격력의 합, 평균, max, min 등을 알 수 있음

* 쿼리문
~~~sql
SELECT
 집계할컬럼1,
 집계함수(COUNT, MAX, MIN 등)
FROM Table
GROUP BY
 집계할컬럼1
~~~

### 그룹화 활용 포인트
  - 일자별 집계
  - 연령대별 집계
  - 특정 타입별 집계
  - 앱 화면별 집계 등등 

### DISTINCT
* 고유값을 알고싶은 경우-> GROUPBY와 비슷하게 중복제거하는 기능이 있음.
* Ex) SELECT 
         집계할 컬럼, COUNT(DISTINCT count할 컬럼)
          ->그 칼럼에서의 distinct한 값만 count해줌
* Ex) SELECT
      DISTINCT 집계할 컬럼
      ->중복값을 제외한 컬럼 내놓음.


### 조건을 설정하는 경우 WHERE, HAVING
  - WHERE: Table에 바로 조건설정-> raw data에서 바로 조건 설정
  - HAVING: GROUP BY한 후에 조건 설정-> 원래 원본 Table에 없었던, GROUP BY 후 생성된 컬럼에 대하여 조건을 설정하고 싶을때 사용

### 서브쿼리 
  - SELECT 문 안에 존재하는 SELECT 쿼리 
  - FROM 절에 또 다른 SELECT 문을 넣을 수 있음
  - 괄호로 묶어서 사용
  - 서브 쿼리를 작성하고, 서브 쿼리 바깥에서 WHERE 조건 설정하는 것 = 서브 쿼리에서 HAVING으로 하는 것 

### 정렬하기 ORDER BY
  - 쿼리의 맨 마지막에 두고, 맨 마지막에만 한번 작성하면 됨(증간에 필요 X)
  - 쿼리: ORDER BY <컬럼> <순서>
   - 순서: DESC(내림차순), OSC(오름차순)

### 출력 개수 제한하기 LIMIT
  - 쿼리문의 맨 마지막에 작성
  - 쿼리문 결과의 row수를 제한




# 2️⃣ 학습 인증란

![alt text](week2.image-1.jpg)





<br><br>



---

# 3️⃣ 확인문제

## 문제 1

> **🧚Q. 포켓몬 마스터 승화는 포켓몬 데이터 조회하는 SQL문에 재미를 느껴서 혼자서 데이터를 조회하는 쿼리문을 짰습니다. 하지만 세 가지의 오류로 다음 코드가 실행이 안된다고 하는데, 각 오류의 위치와 이유를 설명하고, 올바른 쿼리문으로 수정해보세요.**

~~~sql
# 승화의 SQL Query문 
SELECT name AS '포켓몬 이름', ID;
WHERE type = 'Electric';
FROM pokemon;
~~~



~~~
오류1. AS '포켓몬 이름'에서 별칭을 지정할 때는 따음표를 사용하지 않아야한다. 
오류2. ; 은 한 문장의 끝을 의미하기에 마지막 줄에서만 사용해야 한다.
오류3. 쿼리문 작성은 SELECT -> FROM -> WHERE 순이다.
~~~
~~~sql
SELECT name AS 포켓몬 이름, ID
FROM pokemon
WHERE type = 'Electric'
~~~



## 문제 2

> **🧚Q. 앞서 SQL Query의 오류를 해결한 승화는 기분 좋게 이번에는 포켓몬 데이터에서 타입별 평균 공격력이 60 이상인 타입만 조회하려는 쿼리를 작성하려고 했습니다. 하지만 이번에도 실수를 하여 쿼리문이 실행되지 않거나 잘못된 결과가 나오고 있는데, 쿼리에서 잘못된 부분이 무엇인지 설명하고, 올바르게 수정한 쿼리를 작성해보세요.**

~~~sql
SELECT type, AVG(attack) AS avg_attack
FROM pokemon
WHERE AVG(attack) >= 60
GROUP BY type;
~~~



~~~
 WHERE은 raw data에 한해서 조건을 주는 역할이기 때문에 그룹화 결과로 생겨난 컬럼인 avg_attack에 조건을 붙이려면 HAVING을 써야한다. 
~~~

~~~sql
SELECT type, AVG(attack) AS avg_attack
FROM pokemon
GROUP BY type
HAVING avg_attack >= 60;
~~~


### 🎉 수고하셨습니다.
