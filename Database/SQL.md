# SQL

### SQL(Structured Query Language)이란?
- 관계 데이터베이스를 위한 표준 질의어로, 비절차적 언어(선언적 언어)이다.
- 사용자는 자신이 원하는 바(what)만 명시하며, 원하는 것을 처리하는 방법(how)은 명시할 수 없다.

<br/>

#### SQL의 분류
- `DDL`: 데이터 정의어
  - 테이블을 생성하고 변경,제거하는 기능을 제공

<br/>

- `DML`: 데이터 조작어
  - 테이블에 새 데이터를 삽입하거나, 테이블에 저장된 데이터를 수정,삭제,검색하는 기능을 제공

<br/>

- `DCL`: 데이터 제어어
  - 보안을 위해 데이터에 대한 접근 및 사용 권한을 사용자별로 부여하거나 취소하는 기능을 제공

<br/>


<br/>

> ## DDL
### 💡 테이블 생성
- SQL 질의문은 세미콜론(;)으로 문장의 끝을 표시하고, 대소문자를 구분하지 않는다.
- 아래 코드에서 [ ]의 내용은 생략이 가능하다.
    ```sql
    CREATE TABLE 테이블_이름 (
        속성_이름 데이터_타입 [NOT NULL] [DEFAULT 기본_값],
        [PRIMARY KEY (속성_리스트)],
        [UNIQUE (속성_리스트)],
        [FOREIGN KEY (속성_리스트) REFERENCES 테이블_이름(속성_리스트)], 
        [CONSTRAINT 이름] [CHECK(조건)]
    );
    ```

<br/>

### 💡 테이블 변경
- 새로운 속성의 추가, 기존 속성의 삭제, 새로운 제약조건의 추가/삭제 등이 가능하다.
- 새로운 속성의 추가
    ```sql
    ALTER TABLE 테이블_이름
      ADD 속성_이름 데이터_타입 [NOT NULL] [DEFAULT 기본_값];
    ```

<br/>

- 기존 속성의 삭제
    ```sql
    ALTER TABLE 테이블_이름 
     DROP COLUMN 속성_이름;
    ```

<br/>

- 새로운 제약 조건의 추가
    ```sql
    ALTER TABLE 테이블_이름 
      ADD CONSTRAINT 제약조건_이름 제약조건_내용;
    ```

<br/>

- 기존 제약조건의 삭제
    ```sql
    ALTER TABLE 테이블_이름 
     DROP CONSTRAINT 제약조건_이름;
    ```

<br/>

### 💡 테이블 삭제
- 테이블을 삭제할 수 있다.
- 만약, 삭제할 테이블을 참조하는 테이블이 있다면? → 삭제가 수행되지 않는다.
- 이때는, 관련된 외래키 제약조건을 먼저 삭제해야 한다.
    ```sql
    DROP TABLE 테이블_이름;
    ```

<br/>

<br/>

> ## DML
### 💡 데이터 검색

<br/>

#### 기본 검색
- `SELECT` 키워드와 함께 검색하고 싶은 속성의 이름을 나열한다.
- FROM 키워드와 함께 검색하고 싶은 속성이 있는 테이블의 이름을 나열한다.
- 검색 결과는 테이블 형태로 반환된다.
    ```sql
    SELECT [ALL | DISTINCT] 속성_리스트
      FROM 테이블_리스트;
    ```

<br/>

- `AS` 키워드를 이용해 결과 테이블에서 속성의 이름을 바꾸어 출력도 가능하다.
- 새로운 이름에 공백이 포함되어 있으면 큰따옴표나 작은따옴표로 묶어주어야 한다.
    ```sql
    SELECT [ALL | DISTINCT] 속성_리스트 AS 새로운_이름
      FROM 테이블_리스트;   
    ```

<br/>

#### 산술식을 이용한 검색
- SELECT 키워드와 함께 산술식을 제시할 수 있다.
- 속성의 이름과 +,-,*,/ 등의 산술 연산자와 상수로 구성된다.
    ```sql
    SELECT [ALL | DISTINCT] 속성_리스트 + 1000 AS 새로운_이름
      FROM 테이블_리스트;   
    ```

<br/>

#### 조건 검색
- 조건을 만족하는 데이터만 검색한다.
    ```sql
    SELECT [ALL | DISTINCT] 속성_리스트
      FROM 테이블_리스트
     WHERE 조건;
    ```

<br/>

#### LIKE를 이용한 검색
- `LIKE`키워드를 이용해 부분적으로 일치하는 데이터를 검색한다.
  - 문자열을 이용하는 조건에만 `LIKE` 키워드 사용이 가능하다.
  - `LIKE` 키워드와 함께 사용할 수 있는 기호
    - % : 0개 이상의 문자
    - _ : 1개의 문자
  ```sql
  -- 김씨 성을 가진 직원의 직급 구하기
  SELECT empname, title 
    FROM employee
   WHERE empname LIKE '김%';
  ```

<br/>

#### `NULL`을 이용한 검색
- `IS NULL` 키워드를 이용해 특정 속성의 값이 널 값인지를 비교한다.
- `IS NOT NULL` 키워드를 이용해 특정 속성의 값이 널 값이 아닌지를 비교한다.
    ```sql
    -- 이름이 입력된 직원들 조회
    SELECT empname 
      FROM employee
     WHERE empname IS NOT NULL;
    ```

<br/>

#### 정렬 검색
- `ORDER BY` 키워드를 이용해 결과 테이블 내용을 사용자가 원하는 순서로 출력한다.
- 오름차순(기본) : ASC, 내림차순: DESC
    ```sql
    SELECT [ALL | DISTINCT] 속성_리스트
      FROM 테이블_리스트
     WHERE 조건
     ORDER BY 속성_리스트 [ASC | DESC];
    ```

<br/>

#### 집계 함수를 이용한 검색
- 특정 속성 값을 통계적으로 계산한 결과를 검색하기 위해 집계 함수를 이용한다.
    - 개수, 합계, 평균, 최댓값, 최솟값의 계산 기능을 제공한다.
- 집계 함수 사용 시 주의 사항
    - 집계 함수는 널인 속성 값은 제외하고 계산한다.
    - 집계 함수는 `WHERE` 절에서는 사용할 수 없고, `SELECT` 절이나 `HAVING` 절에서만 사용 가능하다.
    ```sql
    -- 모든 사원들에 대해서 사원들 중 평균 급여가 2,500,000원 이상인 부서에 대하여 부서번호, 평균 급여와 최대 급여를 검색
    SELECT dno AS "부서 번호", AVG(salary) AS "평균 급여", MAX(salary) AS "최대 급여" 
      FROM employee
    HAVING AVG(salary) >= 2500000;
    ```

<br/>

#### 그룹별 검색
- `GROUP BY` 키워드를 이용해 특정 속성의 값이 같은 투플을 모아 그룹을 만들고, 그룹별로 검색한다.
- `HAVING` 키워드를 함께 이용해 그룹에 대한 조건을 작성한다.
  ```sql
  SELECT [ALL | DISTINCT] 속성_리스트
    FROM 테이블_리스트
   WHERE 검색조건
   GROUP BY 속성_리스트
  HAVING 조건;
  ```

<br/>

#### 여러 테이블에 대한 조인 검색
- 조인 검색 : 여러 개의 테이블을 연결하여 데이터를 검색하는 것
- 조인 속성 : 조인 검색을 위해 테이블을 연결해주는 속성
    - 연결하려는 테이블 간에 조인 속성의 이름은 달라도 되지만, 도메인은 같아야 한다.
    - 일반적으로 외래키를 조인 속성으로 이용한다.
- `FROM` 절에 검색에 필요한 모든 테이블을 나열한다.
- `WHERE` 절에 조인 속성의 값이 같아야 함을 의미하는 조인 조건을 제시한다.
    ```sql
    -- 고객ID에 문자 ‘j’를 포함한 사람의 고객ID, 주소, 고객명, 등급명, 할인율을 검색하시오.
    SELECT C.c_id, C.c_address, C.c_name, C.c_code, CG.cg_discount 
      FROM s_customer C, s_customer_grade CG
     WHERE c_id LIKE '%j%'
       AND C.c_code = CG.cg_code;
    ```

<br/>

#### INNER JOIN / OUTTER JOIN
- 조인 조건을 만족하지 않는 투플에 대해서도 검색을 수행하려면 외부 조인을 요청하면 된다.
    ```sql
    SELECT 속성_리스트
      FROM 테이블1 LEFT | RIGHT | FULL OUTER JOIN 테이블2 ON 조인조건
     WHERE 검색조건;
    ```

<br/>

#### 부속 질의문을 이용한 조인 검색
- `SELECT` 문 안에 또 다른 `SELECT` 문을 포함하는 질의
    - 단일 행 부속 질의문 : 하나의 행을 결과로 반환한다.
    - 다중 행 부속 질의문 : 하나 이상의 행을 결과로 반환한다.
- 부속 질의문과 상위 질의문을 연결하는 연산자가 필요하다.
    - 단일 행 부속 질의문은 비교 연산자 사용 가능
    - 다중 행 부속 질의문은 비교 연산자 사용 불가능
  ```sql
  -- 영업부나 개발부에 근무하는 사원들의 이름을 검색하라
  SELECT empname 
    FROM employee
   WHERE dno IN (SELECT deptno 
                   FROM department
                  WHERE deptname IN ('영업', '개발'));
  ```

<br/>

- 다중 행 부속 질의문에 사용 가능한 연산자
  - `IN` : 부속 질의문의 결과 값 중 일치하는 것이 있으면 참
  - `NOT IN` : 부속 질의문의 결과 값 일치하는 것이 없으면 참
  - `EXISTS` : 부속 질의문의 결과 값이 하나라도 존재하면 검색 조건이 참
  - `NOT EXISTS` : 부속 질의문의 결과 값이 하나도 존재하지 않으면 검색 조건이 참
  - `ALL` : 부속 질의문의 결과 값 모두와 비교한 결과가 참이면 검색 조건을 만족
  - `ANY` 또는 `SOME` : 부속 질의문의 결과 값 중 하나라도 비교한 결과가 참이면 검색 조건을 만족

<br/>

#### 상관 중첩 질의(correlated nested query)
- 중첩 질의의 `WHERE`절에 있는 프레디키트에서 외부 질의에 선언된 릴레이션의 일부 속성을 참조하는 질의
- 외부 질의를 만족하는 각 투플이 구해진 후에 중첩 질의가 수행되므로, 상관 중첩 질의는 외부 질의를 만족하는 투플 수만큼 여러 번 수행될 수 있다.
    ```sql
    -- 2022년 3월 15일에 제품을 주문한 고객의 고객이름을 검색하시오
    SELECT customer_name
      FROM customer
     WHERE EXISTS (SELECT *
                     FROM porder
                    WHERE order_date = '2022-03-15'
                      AND porder.customer_id = customer.customer_id);
    ```

<br/>

<br/>

### 💡 데이터 삽입
- `INTO` 키워드와 함께 투플을 삽입할 테이블의 이름과 속성의 이름을 나열한다.
- `VALUES` 키워드와 함께 삽입할 속성 값들을 나열한다.
    ```sql
    INSERT 
      INTO 테이블_이름[(속성_리스트)]
    VALUES (속성값_리스트);
    ```

<br/>

<br/>

### 💡 데이터 수정
- `SET` 키워드 다음에 속성 값을 어떻게 수정할 것인지를 지정한다.
- `WHERE` 절에 제시된 조건을 만족하는 투플에 대해서만 속성 값을 수정한다.
    ```sql
    UPDATE 테이블_이름
       SET 속성_이름1 = 값1, 속성_이름2 = 값2, ...
     WHERE 조건;
    ```

<br/>

### 💡 데이터 삭제
- 테이블에 저장된 데이터를 삭제한다.
    ```sql
    DELETE
      FROM 테이블_이름
     WHERE 조건;
    ```
