---
layout: post
title: Oracle Database[2]-함수
---

## 함수
### 함수에 대해 알아보자

- 하나의 큰 프로그램에서 반복적으로 사용되는 부분들을 분리하여 작성해 놓은 작은 서브 프로그램
- 호출하며 값을 전달하면 결과를 리턴하는 방식으로 사용
- 단일 행 함수
    - 테이블에 있는 데이터 (ROW)를 각각 실행해서 결과를 반환해주는 함수 -> ROW의 객수만큼 결과를 반환한다.
    - 단일행함수는 처리되는 타입에 따라 문자 처리함수, 숫자처리함수, 날짜처리함수로 나눌 수 있다.
    - SELECT문의 컬럼 작성위치, SELECT문 WHERE절 작성위치에 사용이 가능하다.
    - INSERT, UPDATE, DELETE문에서도 사용이 가능하다.
- 그룹 함수
    - 테이블에 있는 데이터(ROW)를 종합하여 한개의 결과를 반환해주는 함수 -> 결과가 한개 행이 반환된다.(평균, 총합, 갯수)

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
        - TRIM(LEADING||TRAILING||BOTH '문자' FROM '문자열'||컬럼)
    - SUBSTR
        - JAVA의 SUBSTRING이랑 동일한 기능, 문자열을 인덱스기준으로 잘라내는 것
        - SUBSTR('문자열'||시작인덱스[,길이])
    - UPPER, LOWER INITCAP
        - 컬럼의 문자 혹은 문자열을 소문자/대문자/첫 글자만 대문자로 변환하여 반환
        - UPPER - 대문자로 변환(LIKE 연산자 사용시 대소문자 상관없이 검색할 때 사용)
        - LOWER - 소문자로 변환(LIKE 연산자 사용시 대소문자 상관없이 검색할 때 사용)
        - INITCAP - 첫번째 문자를 대문자로 변환
        
        ```sql
        SELECT 
            UPPER('Welcom to oRACLEworld'), 
            LOWER('Welcom to oRACLEworld'),
            INITCAP('Welcom to oRACLEworld')
        FROM DUAL;
        
        -- UPPER : 'WELCOME TO ORACLEWORLD'
        -- LOWER : 'welcome to oracleworld'
        -- INTICAP : 'Welcome To Oracleword'
        ```
    - CONCAT
        - 연결연산 / 문자열을 연결해주는 연산 : 컬럼의 문자 혹은 문자열을 두 개 이상 전달받아 하나로 합친 후 반환
        
        ```sql
        SELECT
            CONCAT('ORACLE','DATABASE')
        FROM
            DUAL;
        ```
    - REPLACE
        - 대상문자(컬럼)에서 지정문자를 찾아 대체문자로 변경하는 것 - 일괄적으로 데이터를 바꾸어야 할 경우에 용이
        - REPLACE(대상문자 || 컬럼, 지정문자. 대체문자)
        
        ```sql
        SELECT '나는 오라클을 싫어해', REPLACE('나는 오라클을 싫어해','싫어','좋아')
        FROM DUAL;
        
        -- 싫어를 좋아로 변경
        ```
    - REVERSE
        - 해당문자열을 역순으로 만드는 것
        
        ```sql
        SELECT 'ABCDE', REVERSE('ABCDE')
        FROM DUAL;
        
        -> EDCBA
        ```
    - TRANSLATE
        - 해당문자를 매칭되어있는 문자로 출력해주는 기능
        
        ```sql
        SELECT TRANSLATE('010-3644-6259','0123456789','영일이삼사오육칠팔구')
        FROM DUAL;
        
        -> 영일영-삼육사사-육이오구
        ```
   
   
2. 숫자 처리 함수
    - ABS
        - 절대값을 구하는 함수
        - 양수, 음수 등 어떤 숫자를 넣어도 절대값만 출력
        
        ```sql
        SELECT ABS(-10), ABS(10)
        FROM DUAL;
        
        -> 10, 10
        ```
    - MOD
        - MOD(a,b) : a를 b로 나누었을때 나머지 값을 반환
        
        ```sql
        SELECT MOD(3,2)
        FROM DUAL;
        
        -> 1
        ```
    - ROUND
        - 특정 소수점을 반올림하고 나머지를 버리는 함수
        - ROUND(소수점숫자[,자리수])
        - -를 붙이명 해당 자리수의 값을 0으로 만든다.
        
        ```sql
        SELECT 
            ROUND(126.456), -- 소수점을 기준으로 반올림
            ROUND(126.543), -- 소수점을 기준으로 반올림
            ROUND(126.456,2), -- 소수점 2자리까지 반올림해서 출력 
            ROUND(126.456,-2) -- 소수점으로부터 앞으로 2자리까지 0으로 변환
        FROM DUAL;
        
        -> 126, 127, 126.46, 100
        ```

    - FLOOR
        - 반올림을 하지 않고 소수점를 자리 버린다.
        
        ```sql
        SELECT FLOOR(126.456),FLOOR(126.543)
        FROM DUAL;
        
        -> 126, 126
        ```
    -  TRUNC
        - 자리수를 지정 소수점자리 버린다.
        
        ```sql
        SELECT 
            TRUNC(123.456), -- 소수점 자르기
            TRUNC(123.456, 2) -- 소수점 2자리 남기고 자르기
        FROM DUAL;
        ```
     - CEIL
        - 반올림이 아니라 무조건 올림
        
        ```sql
        SELECT CEIL(126.456), CEIL(126.567)
        FROM DUAL;
        
        -> 127, 127
        ```
3. 날짜 처리 함수
    - SYSDATE
        - 현재 날짜 출력
        - SYSDATE : 년월일시분초 까지 표시
        - SYSDATESTAMP : 년월일시분초 밀리세컨드 까지 표시
        
        ```sql
        SELECT SYSDATE, SYSTIMESTAMP
        FROM DUAL;
        
        -> 23/02/13, 23/02/13 12:54:13.191000000 +09
        ```
    - NEXT_DAY
        - 매개변수로 받은 요일중 가장 가까운 다음 일자를 표시
        
        ```sql
        SELECT NEXT_DAY(SYSDATE, '화요일'), NEXT_DAY(SYSDATE, '금')
        FROM DUAL;
        ```
    - LAST_DAY
        - 현재 달의 마지막 날 출력
        
        ```sql
        SELECT LAST_DAY(SYSDATE)
        FROM DUAL;
        ```
    - ADD_MONTHS
        - 지정된 날에서 개월수를 더하는 계산
        ```sql
        SELECT 
            ADD_MONTHS(SYSDATE,3), -- 해당 달에서 3개월을 더해서 출력
            ADD_MONTHS(SYSDATE,10) -- 해당 달에서 10개월을 더해서 출력
        FROM DUAL;
        ```
    - MONTHS_BETWEEN
        - 두개 날짜를 받아서 두 날짜의 개월수 차이를 계산해주는 기능
        
        ```sql
        SELECT MONTHS_BETWEEN(SYSDATE,'22/01/01')
        FROM DUAL;
        
        -- 별거 아닌 응용
        SELECT TRUNC(MONTHS_BETWEEN(SYSDATE,'22/01/01'))
        FROM DUAL;
        
        -- TRUNC를 사용해서 소수점을 버린 개월수를 출력할 수 있다.
        ```
    - EXTRACT
        - 날짜의 년, 월, 일을 따로 출력할 수 있게 해주는 함수
        - EXTRACT(YEAR FROM 날짜) : 날짜에서 년을 출력
        - EXTRACT(MONTH FROM 날짜) : 날짜에서 월을 출력
        - EXTRACT(DAY FROM 날짜) : 날짜에서 일을 출력
        - 숫자형으로 반환
        
        ```sql
        SELECT 
            EXTRACT(YEAR FROM SYSDATE), -- 해당 날짜에서 년도만 출력
            EXTRACT(MONTH FROM SYSDATE), -- 해당 날짜에서 월만 출력
            EXTRACT(DAY FROM SYSDATE) -- 해당 날짜에서 일만 출력
        FROM DUAL;
        ```
4. 형변환 함수
    - TO_CHAR : 입력값 타입 = DATE, NUMBER -> 리턴값 타입 =CHARACTER
        - 날짜형 혹은 숫자형을 문자형으로 변환
    - TO_DATE : 입력값 타입 = CHARACTER, NUMBER -> 리턴값 타입 = DATE
        - 문자형 혹은 숫자형을 날짜형으로 변환
    - TO_NUMBER : 입력값 타입 = CHARACTER -> 리턴값 타입 = NUMBER
        - 문자형을 숫자형으로 변환
    































