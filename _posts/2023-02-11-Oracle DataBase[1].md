---
layout: post
title: Oracle Database[1]
---

### SQL

- SQL(Structured Query Language)<br>
관계형 데이터베이스에서 데이터를 조회하거나 조작하기 위해 사용하는 표준 검색 언어로 원하는 데이터를 찾는 방법이나 절차를 기술하는 게 아닌 조건을 기술하하여 작성
    - DML(Data Manipulation Language) : 조작어(데이터 조회 및 변형을 위한 명령어) : SELECT, INSERT, UPDATE, DELETe
    - DDL(Data Definition Language) : 정의어(데이터의 구조를 정의 하기 위한 테이블 생성,삭제 같은 명령어) : CREATE, DROP, ALTER
    - DCL(Data Control Language) : 제어어(사용자에게 권한 생성 혹은 권한 삭제 같은 명령어) : COMMIT, ROLLBACK, GRANTE, REVOKE
    -------------
    - DQL(Data Query Language) : 데이터 검색 : SELECT -> 일부에서는 DML에서 SELECT 만을 따로 분리해서 DQL (Data Query Language) 라고 표현
    - TCL(Transaction Control Language) : 트랜잭션 제어 : COMMIT, ROLLBAC -> 일부에서는 DCL 에서 트랜잭션을 제어하는 명령인 COMMIT 과 ROLLBACK 만을 따로 분리해서 TCL (Transaction Control Language) 라고 표현

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













