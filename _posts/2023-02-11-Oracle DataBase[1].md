---
layout: post
title: Oracle Database[1]
---

### SQL

- SQL(Structured Query Language)
    - 관계형 데이터베이스에서 데이터를 조회하거나 조작하기 위해 사용하는 표준 검색 언어로 원하는 데이터를 찾는 방법이나 절차를 기술하는 게 아닌 조건을 기술하하여 작성

    |분류|용도|명령어|
    |------|---|---|
    |DQL(Data Query Language)|데이터 검색|SELECT|
    |DML(Data Manipulation Language)|데이터 조작|INSERT, UPDATE, DELETE|
    |DDL(Data Definition Language)|데이터 정의|CREATE, DROP, ALTER|
    |TCL(Transaction Control Language)|트랜잭션 제어|COMMIT, ROLLBAC|

- 주요 데이터 타입
    
    <table >
        <tr align = "center" >
            <th>데이터 타입</th>
            <th>하위 데이터 타입</th>
            <th>설명</th>
        </tr>
        <tr align = "center">
            <td>NUMBER</td>
            <td></td>
            <td>숫자</td>
        </tr>
        <tr align = "center">
            <td rowspan="3" align = "center">CAHARACTER</td>
            <td>VARCHAR2</td>
            <td>가변길이 문자(최대 4000바이트)</td>
        </tr>
        <tr align = "center">
            <td>VARCHAR2</td>
            <td>가변길이 문자(최대 4000바이트)</td>
        </tr>
        <tr align = "center">
            <td>LONG</td>
            <td>가변길이 문자(최대 2기가 바이트)</td>	
        </tr>
        <tr align = "center">
            <td>DATE</td>
            <td></td>
            <td>날짜</td>
        </tr>
        <tr align = "center">
            <td rowspan="3" align = "center">LOB</td>
            <td>CLOB</td>
            <td>가변길이 문자(최대 4기가 바이트)</td>
        </tr>
        <tr align = "center">
            <td>BLOB</td>
            <td>Binary Data</td>	
        </tr>
    </table>
