---
layout: post
title: Oracle Database[2]
---

## 함수
### 함수에 대해 알아보자

- 하나의 큰 프로그램에서 반복적으로 사용되는 부분들을 분리하여 작성해 놓은 작은 서브 프로그램
- 호출하며 값을 전달하면 결과를 리턴하는 방식으로 사용
- 단일 행 함수 : 테이블에 있는 데이터 (ROW)를 각각 실행해서 결과를 반환해주는 함수 => ROW의 객수만큼 결과를 반환한다.
    - 단일행함수에 대해 알아보자
    - 단일행함수는 처리되는 타입에 따라 문자 처리함수, 숫자처리함수, 날짜처리함수로 나눌 수 있다.
    - SELECT문의 컬럼 작성위치, SELECT문 WHERE절 작성위치에 사용이 가능하다.
    - INSERT, UPDATE, DELETE문에서도 사용이 가능하다.
- 그룹 함수 : 테이블에 있는 데이터(ROW)를 종합하여 한개의 결과를 반환해주는 함수 -> 결과가 한개 행이 반환된다.(평균, 총합, 갯수)

1. 문자 처리 함수
    - LENGTH : 주어진 컬럼 값/문자열의 길이(문자 개수) 반환
        - LENGTH(컬럼명 || '리터럴') -> 리턴값 NUMBER
        - 컬럼을 이용
            ```sql
            SELECT EMP_NAME, LENGTH(EMP_NAME) FROM EMPLOYEE;
            ```
        - WHERE절에서 사용
            ```sql
            SELECT EMP_NAME, EMAIL, LENGTH(EMAIL)
            FROM EMPLOYEE
            WHERE LENGTH(EMAIL) >= 16;
            ```
    - LENGTHB : 주어진 컬럼 값/문자열의 길이(BYTE) 반환
        - LENGTHB(CHAR | STRING) -> 리턴값 CHARACTER
    - INSTR
        - 찾는 문자열이 지정한 위치(인덱스)부터 지정한 횟수번째 나타나는 위치(인덱스)를 반환해주는 함수
        - INSTR(문자열||컬럼명(대상),문자열||컬럼명(찾을문자열)[,시작인덱스위치, 몇번째]
        ```sql
        -- 오라클에서 인덱스는 1부터 시작함.
        SELECT INSTR('GD아카데미 GD소프트웨어개발 GD경영','GD'),
                INSTR('GD아카데미 GD소프트웨어개발 GD경영','GD',3),
                INSTR('GD아카데미 GD소프트웨어개발 GD경영','GD',3,2),
                INSTR('GD아카데미 GD소프트웨어개발 GD경영','GD',-1,2) -- 음수는 오른쪽에서 시작!
        FROM DUAL;
        ```
    - LPAD/RPAD 
        - 왼쪽, 오른쪽 공백에 특정문자를 추가해주는 함수
        - LPAD('문자열'||컬럼명, 차지할 길이,채울문자)
        - 채울문자를 생략하면 공백이 자동으로 들어감
        ```sql
        SELECT '오라클', LPAD('오라클',10,'*'),LPAD('오라클',6,'*')
        FROM DUAL;
        ```
    - LTRIM/RTRIM
        - 문자열의 왼쪽, 오른쪽에 공백을 제거하거나 , 특정 문자를 제거하는 함수
        - L/RTRIM(문자열||컬럼명[,문자])
    - TRIM
        - 양쪽에 있는 값을 제거하는 함수, 기본 공백, 지정한 문자(한개만)를 제거
        - TRIM(문자열||컬럼명) -> 공백을 제거함
        - TRIM('한개문자'FROM 문자열||컬럼명) ->  지정한 문자제거



