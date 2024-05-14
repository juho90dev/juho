---
layout: post
title: Spring Cloud[1]
---

## Spring Cloud
- 마이크로서비스의 개발, 배포, 운영에 필요한 아키텍처를 쉽게 구성할 수 있도록 지원하는 Spring Boot기반의 프레임워크
- "MSA구성을 지원하는 Springboot기반 Framework"
- RAD(Rapid Application Development)방법론을 채택하여 Spring Framework와 같이 Java 기반으로 개발
    - RAD란?
    - 개발 방법론 중 하나로 ‘빠르게 애플리케이션을 개발’ 하기 위한 방법론
    - 개발 생명주기의 단축과 빠른 프로ㅁ MA)
    - 애플리케이션을 하나의 서비스 혹은 애플리케이션이 '하나의 통합된 구조'를 갖는 것을 의미
    - 설멍 : 모든 서비스를 단일 어플리케이션에 통합
    - 배포 : '전체' 어플리케이션 배포 필요
    - 확장성 : '수직 확장'만 가능 : Scale-In(서버의 성능을 향상하는 것으로 단일 서버에 더 많은 하드웨어 자원을 추가하는 것)
    - 개발 방식 : 단일 코드 베이스
    - 유지 보수 : 전체 어플리케이션을 수정해야 함
    - 복잡성 : 쉽게 이해할 수 있음
    - 테스트 : 전체 어플리케이션 테스트 필요
    - 데이터 관리 : 중앙 집중식 데이터 관리
    - 장애 처리 : 전체 시스템 다운

2. 마이크로 서비스 아키텍처(Micro Service Architecture : kMSA)
    - 하나의 애플리케이션을 독립적으로 배포 가능한 서비스 단위로 분할을 하여 '서로 간의 변경과 조합이 가능하도록 이루는 구조'를 갖는 것을 의미
    - 설멍 : 서비스를 작은 단위로 분리하여 독립적으로 배포하고 운영
    - 배포 : '필요한 부분'만 배포 가능
    - 확장성 : '수평 확장' 가능 : Scale-Out(서버의 용량을 늘리는 것으로 ‘서버 인스턴스’를 추가하여 부하를 분산시키는 것)
    - 개발 방식 : 독립적인 코드 베이스
    - 유지 보수 : 각 서비스를 독립적으로 수정 가능
    - 복잡성 : 분산 시스템으로 인한 복잡성
    - 테스트 : 각 서비스를 독립적으로 테스트 가능
    - 데이터 관리 : 서비스 간 데이터 공유가 어려움
    - 장애 처리 : 일부 서비스만 다운되므로 장애 발생 시 영향 최소화 가
  
3. Spring Boot와 Spring Cloud
    - Spring Boot
        - 종류 : '단일 애플리케이션 개발'을 위한 프레임 워크
        - 장점 : 빠른 개발과 배포, 간단한 설정, Spring 기반으로 다양한 라이브러리 지원
        - 단점 : 대규모 분산 시스템에서는 한계가 있음
        - 주요 모듈 : Spring MVC, Spring Data, Spring Security, Spring Boot Actuator
    - Spring Cloud
        - 종류 : 분산 시스템의 개발 및 운영'을 위한 프레임 워크(Spring Boot에서 제공하느 기능을 기반으로 분산 시스템에서 필요한 다양한 기능들을 추가로 제공)
        - 장점 :  Spring Boot에서 제공하느 기능을 기반으로 분산 시스템에서 필요한 다양한 기능들을 추가로 제공
        - 단점 : 설정이 복잡하고 러닝커브가 높다
        - 주요 모듈 : Spring Cloud, Config, Discovery, Routing, Circuit Brreaker, Messing
4. Netflix OSS(Netflix Open Source Softwara)
    - Netflix에서 개발한 오픈소스 소프트웨어들의 집합으로 클라우드 네이티브 애플리케이션을 만들기 위한 다양한 도구들을 자체적으로 사용하면서 성능이 검증된 라이브러리로 제공
    - 대표적이 도구로 Eureka, Hystrix, Ribbon, Zuul 등이 있다.
        - Eureka
            - 서비스 디스커버리 도구
            - 각가의 서비스 인스턴스들을 등록하고 관리하는 역할 
        - Hystrix
            - 서킷 브레이커 패턴 라이브러리
            - 서비스 간의 의존성을 관리하고 서비스 장애 시에도 전체 시스템의 안전성을 유지
        - Ribbon
            - 클라이언트 측 로드 밸런싱 라이브러리
            - 여러 개의 인스턴스 중에서 가장 적은 부하를 가진 인스턴스를 선택하여 요청을 보내는 역할     
        - Zuul
            - API 게이트웨이
            - 클라이언트와 서비스 사이에서 라우팅, 필터링, 보안 등의 역할을 수행

## Spring Cloud 종류
1. 주요 컴포넌트
    - **Spring Cloud**
        - Cloud Bootstrap : Spring Cloud 애플리케이션을 시작하기 위한 기본 구성 요소를 제공
        - Function : Spring Cloud Function을 사용하여 서버리스 기능을 작성하고 실행할 수 있는 기능을 제공
        - Task
    - **Spring Cloud Tools (Spring Tool Suite용 플러그인과 확장 기능)**
        - Open Service Broker : 서비스 브로커를 생성하기 위한 도구를 제공
    - **Spring Cloud Config : 분산 시스템을 위한 중앙 집중식 구성 관리를 제공**
        - Config Client
        - Config Server
        - Valut Configuration
        - Apache Zookeeper Configuration
        - Consul Configuration 











