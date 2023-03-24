---
layout: post
title: Oracle DataBase[3]-GROUP BY & HAVING & ORDER BY
---

## GROUP BY & HAVING & ORDER BY에 대해 간단히 정리해 보자.


1. GROUP BY
    - 데이터를 특정 컬럼 기준으로 그룹화시키는 명령어
    - 그룹함수를 사용했을때 특정 그룹을 지정해서 여러값을 출력하게 해주는 명령어
    - GROUP BY 컬럼명 -> 컬럼값이 동일한 것끼리 묶어서 그룹화시켜줌.
  
    ```sql
    SELECT 컬럼명, 컬럼명
    FROM 테이블명
    WHERE 조건문
    GROUP BY 컬럼명
    ```
2. HAVING
    - 그룹 함수로 값을 구해올 그룹에 대해 조건을 설정할 때 HAVING절에 기술
    - SELECT의 조건은 WHERE절, GROUP BY절의 조건은 HAVING절이다.
    - HAVING절이 있을 떄의 수행되는 순서
        1. FROM : 조회할 테이블을 먼저 찾는다.
        2. WHERE : 출력 대상 데이터가 아닌 것은 제거 -> SELECT에 대한 조건절
        3. GROUP BY : 행들을 그룹화하고
        4. GROUP FUNCTION : 그룹함수를 적용한다.
        5. HAVING : 마지막으로 그룹함수값의 조건에 맞는 것만 발췌하여 출력.
  
3. ORDER BY
    - SELECT한 컬럼에 대해 `정렬`을 할 때 작성하는 구문
    - SELECT 구문의 가장 마지막에 작성하며 실행 순서 또한 가장 마지막에 수행됨
    - 정렬방식
        - ASC : 오름차순
        - DESC : 내림차순
    
    ```sql
    SELECT 컬럼 명 [, 컬럼명, …] 
    FROM 테이블 명
    WHERE 조건식
    ORDER BY 컬럼명 | 별칭 | 컬럼 순번 정렬방식 [NULLS FIRST | LAST];
    ```
  
  
  
  
4. ROLLUP / CUBE
    - GROUP BY절에 사용함.
    - 전체데이터의 총합을 계산해주는 기능.
    - ROLLUP
        - 순차적으로 중간 합계를 출력
        - 가장 먼저 지정한 그룹별로 추가적 집계 결과 반환하기 때문에 지정한 값의 순서가 다르면 결과값이 달라진다.
    - CUBE
        - CUBE는 모든 중간 합계를 출력한다.
        - ROLLUP 함수와는 다르게 인자의 순서가 달라도 결과는 같다.


5. 집합연산자
    - 두개이상의 SELECT문을 합쳐주는 연산자
        - UNION : 두개 이상의 SELECT문을 합쳐주는 연산  * 중복값을 한개만 표시
        - UINON ALL : 두개이상의 SELECT문을 합쳐주는 연산  * 중복값을 모두 표시
        - INTERSECT : 두개이상의 SELECT문에서 중복되는 값만 보여주는 연산
        - MINUS : 두개이상의 SELECT문에서 기준SELECT문에서 다른 SELECT문의 결과를 밴 나머지만 보여주는 연산 * 중복 제거 







