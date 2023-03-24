---
layout: post
title: Oracle DataBase[4] - JOIN
---

## JOIN

여러테이블을 특정컬럼값을 기준으로 합쳐서 한개 테이블로 사용하게 하는 것.
오라클 전용 구문과 ANSI표준이 있다.
- 오라클 전용 구문
    - 테이블과 테이블 사이에 ','로 작성한다
    - WHERE절에서 합칠 컬럼을 작성
    
    ```sql
    SELECT DEPT_TITLE, LOCATION_ID, EMP_NAME, EMP_ID
    FROM EMPLOYEE, DEPARTMENT
    WHERE DEPT_CODE=DEPT_ID;

    ```
- ANSI표준
    - 연결에 사용하려는 컬럼 명이 같은 경우 USING() 사용, 다른 경우 ON()을 사용한다.
    
    ```sql
    -- 컬럼명이 같을 때
    SELECT EMP_ID, EMP_NAME, JOB_CODE, JOB_NAME
    FROM EMPLOYEE
    JOIN JOB USING(JOB_CODE);
    
    -- 컬럼명이 다를 때
    SELECT EMP_ID, EMP_NAME, DEPT_CODE, DEPT_TITLE
    FROM EMPLOYEE
    JOIN DEPARTMENT ON(DEPT_CODE = DEPT_ID);
    ```
    
- INNER JOIN과 OUTER JOIN
    - 기본적으로 JOIN은 INNER JOIN이며 두 개 이상의 테이블을 조인할 때 일치하는 갓이 없는 행은 제외된다.
    - OUTER JOIN은 일치하지 않은 값도 포함이 된다.
    - 두개의 테이블에 동일한 컬럼명을 연결을 할때 테이블에 별칭을 부여하거나 테이블명으로 컬럼을 지정
    - 동일한 컬럼명을 비교할때는 USING명령어를 사용할수 있다.
    
        ```sql
        SELECT EMP_ID, EMP_NAME, JOB_NAME, EMAIL, SALARY
        FROM EMPLOYEE JOIN JOB USING(JOB_CODE);
        ```
    - INNER JOIN
        - 동등비교했을때 일치하는 값이 없는 ROW생략
        - WHERE, GROUP BY ~ HAVING ORDER BY 구문을 사용할 수 있다.
        
            ```sql
            SELECT COUNT(*)
            FROM EMPLOYEE JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID);

            SELECT EMP_NAME, DEPT_CODE, DEPT_TITLE, SALARY
            FROM EMPLOYEE
                JOIN DEPARTMENT ON (DEPT_CODE=DEPT_ID)
            WHERE DEPT_TITLE = '총무부';
            ```
        - SELECT * , 컬럼 사용하려면 반드시 별칭이 필요함.
            ```sql
            SELECT E.*, JOB_NAME, J.JOB_CODE
            FROM EMPLOYEE E JOIN JOB J ON(E.JOB_CODE = J.JOB_CODE);
            ```
    - OUTER JOIN
        - 특정테이블을 기준으로 연결되는 값이 없으면 NULL을 출력하고 데이터 전체를 출력
        - LEFT OUTER JOIN
            - 합치기에 사용한 두 테이블 중 왼쪽에 기술된 테이블의 컬럼 수를 기준으로 JOIN
        - RIGHT OUTER JOIN
            - 합치기에 사용한 두 테이블 중 오른쪽에 기술된 테이블의 컬럼 수를 기준으로 JOIN
        - FULL OUTER JOIN
            - 합치기에 사용한 두 테이블이 가진 모든 행을 결과에 포함
 
 
 - SELF JOIN
    - 테이블 한개를 가지고 조인을 만드는것 -> 자기테이블을 자신것과 연결한다.
    - SELF JOIN테이블에 같은 값을 갖는 컬럼이 두개 있어야 함 
    ```sql
    SELECT E.EMP_NAME,E.EMP_ID, M.EMP_NAME,M.EMP_ID
    FROM EMPLOYEE E LEFT JOIN EMPLOYEE M ON E.MANAGER_ID = M.EMP_ID;
    ```
 








