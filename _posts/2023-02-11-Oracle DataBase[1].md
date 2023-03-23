---
layout: post
title: Oracle Database[1]
---

### SQL

- SQL(Structured Query Language)
    - 관계형 데이터베이스에서 데이터를 조회하거나 조작하기 위해 사용하는 표준 검색 언어로 원하는 데이터를 찾는 방법이나 절차를 기술하는 게 아닌 조건을 기술하하여 작성
    - DML(Data Manipulation Language)
        - 조작어(데이터 조회 및 변형을 위한 명령어) : SELECT, INSERT, UPDATE, DELETe
    - DDL(Data Definition Language)
        - 정의어(데이터의 구조를 정의 하기 위한 테이블 생성,삭제 같은 명령어) : CREATE, DROP, ALTER
    - DCL(Data Control Language)
        - 제어어(사용자에게 권한 생성 혹은 권한 삭제 같은 명령어) : COMMIT, ROLLBACK, GRANTE, REVOKE
    -------------
    - DQL(Data Query Language) 
        - 데이터 검색 : SELECT -> 일부에서는 DML에서 SELECT 만을 따로 분리해서 DQL (Data Query Language) 라고 표현
    - TCL(Transaction Control Language)
        - 트랜잭션 제어 : COMMIT, ROLLBAC -> 일부에서는 DCL 에서 COMMIT 과 ROLLBACK 만을 따로 분리해서 TCL (Transaction Control Language) 라고 표현

- 주요 데이터 타입
    - NUMBER : 숫자
    - CHARACTER 
        - CHAR : 고정길이 문자(최대 2000바이트)
        - VARCHAR2 : 가변길이 문자(최대 4000바이트)
        - LONG : 가변길이 문자(최대 2기가 바이트)
    - DATE : 날짜
    - LOB
        - CLOB : 가변길이 문자(최대 4기가 바이트)
        - BLOB : Binary Data
        
--------
- 오라클 계정 생성 : 관리자 권한이 있는 `system`계정으로 `ID`와 `password`를 부여해서 계정을 생성해줘야함

```sql
create user 유저명 identified by 비밀번호 : 기본생성 명령
    
GRANT 부여할 권한들 [, 권한,권한2] to 사용자계정명;
GRANT CONNECT, RESOURCE to 유저명;
```


- 관리자계정 (system)은 DBMS를 전반적으로 관리하는 계정.
    - 사용자조회, 모든 테이블 조회하거나, 각 계정의 테이블도 조회가능
    - 관리자계정에는 DBMS에 대한 정보를 제공하는 테이블(data 저장소) -> dictionary

```sql
SELECT * FROM DICT;
-- 사용자 계정 조회
SELECT * FROM DBA_USERS;

-- 설치된 테이블 확인하기 
SELECT * FROM TAB; -- 계정이 가지고있는 전체 테이블을 조회하는 명령어
```

- SELECT문 이용하기
    - SELECT문은 지정한 테이블에 있는 데이터(ROW) 조회해서 출려해주는 명령어이다.
   - 사용하는 방법

```sql
SELECT 출력을 원하는 컬럼이 있으면 컬럼명을 작성[,컬럼명, 컬럼명] || 전체 컬럼을 조회하려면 * 작성
FROM 테이블명
[WHERE 조건식] 특정 ROW만 출력하고 싶을때 사용, 생략이 가능

ex) EMPLOYEE 테이블의 전체 데이터 조회하기
SELECT * FROM EMPLOYEE;

-- 테이블에 어떤 컬럼이 있는지 확인 
DESC EMPLOYEE; -- Employees 테이블 속성 정보를 보여줌(description)
```

- Dual 테이블
    - 오라클에서 기본연산이나 함수실행시에 SELECT문 실행할때 테스트용 테이블을 제공한다.
    - 간단하게 함수를 이용해서 계산 결과 값을 확인 할 때 사용한다. 
    - 시스템 사용자(sys)가 소유하는 오라클의 표준 테이블
    - 시스템 사용자(sys)가 소유하지만 어느 사용자에서 접근 가능함.
    - 카디널리티(컬럼 수)와 차수()가 모두 1인 dummy 테이블

- SELECT문을 이용해서 산술연산 처리하기
    1. 리터럴값을 이용해서 산술연산 처리하기 
        ```sql
        SELECT 10+20 FROM DUAL;
        SELECT 10+20,30*40,4/5 FROM DUAL;
        ```
    2. 리터럴값과 컬럼을 산술연산하기
        - 산술연산을 하려면 기본적으로 연산하는 컬럼의 자료가 숫자형, 날짜형이어야함.
    3. 컬럼과 컴럼을 산술연산처리하기 
        ```sql
        SELECT SALARY, SALARY*BONUS FROM EMPLOYEE;
        -- 컬럼값이 Null인것과 연산을 하면 연산이 불가능하여 결과는 NULL을 출력한다.
        ```

- SELECT에서 별칭 부여하기
    - AS예약어를 이용해서 사용이 가능하다.
        ```sql
        SELECT 컬럼명 AS 별칭, 컬럼명 AS별칭 FROM 테이블명

        ex)
        SELECT EMP_NAME AS 사원명, EMP_NO AS 주민번호 FROM EMPLOYEE;
        ```
    - 별칭을 부여할 때 AS는 생략 가능하다.
        ```sql
        SELECT EMP_NAME 사원명 , EMP_NO 주민번호, SALARY 월급 FROM EMPLOYEE;
        ```
    - 별칭을 부여할때 특수기호 사용이 가능하다 ->  " "로 감싸주면 된다.
        ```sql
        SELECT EMP_NAME "사 원 명", EMP_NO "주 민 번 호" FROM EMPLOYEE;
        ```

- 중복값을 제거하는 명령어 : DISTINCT
    - DISTINCT는 SELECT문의 맨 앞에 나와야 한다.
    - DISTINCT 뒤에 여러컬럼을 사용하면 여러컬럼이 모두 동일한 값을 중복으로 본다.
    
    ```sql
    SELECT DISTINCT DEPT_CODE FROM EMPLOYEE;
    
    SELECT DISTINCT DEPT_CODE, EMP_NAME FROM EMPLOYEE;
    ```


- 비교 연산자
    - Between And
        - 비교하려는 값이 지정한 범위에 포함되면 TRUE를 리턴하는 연산자, 상한 값과 하한 값의 경계도 포함된다.
        ```sql
        -- 급여를 3500000보다 많이 받고 6000000보다 적게 받는 직원이름과 급여 조회
        
        SELECT EMP_NAME, SALARY
        FROM EMPLOYEE
        WHERE SALARY BETWEEN 3500000 AND 6000000;
        ```
    - LIKE
        - 문자열 패턴을 비교하는 연산자
        - 비교하려는 값이 지정한 특정 패턴을 만족하면 TRUE를 리턴하는 연산자로 ‘%’와 ‘_’를 와일드카드로 사용한다.
        - % : 글자가 0개 이상 아무문자나 다 허용 
        - _ : 글자가 1개 아무 문자나 다 허용 
        - '%주%' -> 문자열에 주이 포함되어있으면 됨, 주로 시작, 주로 끝나도 되고 중간에 주가 있어도 된다.
        - '_주_' -> 무조건 세글자이면서 가운데 주가 들어가야 한다.
        - '__주%' => 세글자 이상이면서 앞에 두글자 이후에 주가 나오는 문자열.
        
        ```sql
        -- ‘전’씨 성을 가진 직원 이름과 급여 조회
        SELECT EMP_NAME, SALARY
        FROM EMPLOYEE
        WHERE EMP_NAME LIKE ‘전%’;
        
        -- 핸드폰의 앞 네 자리 중 첫 번호가 7인 직원 이름과 전화번호 조회
        SELECT EMP_NAME, PHONE
        FROM EMPLOYEE
        WHERE PHONE LIKE ‘_ _ _7%’;
        ```
    - NOT LIKE
        ```sql
        -- ‘이’씨 성이 아닌 직원 사번, 이름, 이메일 조회
        SELECT EMP_ID, EMP_NAME, EMAIL
        FROM EMPLOYEE
        WHERE EMP_NAME NOT LIKE ‘이%’;
        -- WHERE NOT EMP_NAME LIKE ‘이%’;
        ```
    - IS NULL / IS NOT NULL
        ```sql
        -- 관리자도 없고 부서 배치도 받지 않은 직원 조회
        SELECT EMP_NAME, MANAGER_ID, DEPT_CODE
        FROM EMPLOYEE
        WHERE MANAGER_ID IS NULL AND DEPT_CODE IS NULL;
        -- WHERE DEPT_CODE IS NULL AND BONUS IS NOT NULL
        ```
    - NULL값 조회하기
        - NULL은 연산이 불가능하다.
        - NULL값에 대한 비교연산은 따로 제공을 하기 때문에 NULL값을 비교할때는 제공하는 비교연산을 이용하면 된다.
        - IS NULL -> NULL인 값을 찾을때
        - IS NOT NULL -> NULL이 아닌 값을 찾을때
        
        ```sql
        -- 사원중 DEPT_CODE가 NULL인 사원을 조회해보자.
        
        SELECT *
        FROM EMPLOYEE
        WHERE DEPT_CODE IS NULL;
        ```





