---
layout: post
title: Spring Cloud[2]-특징
---


## Spring Cloud 특징
1. 분산/버전 구성(Distributed/versioned configuration) : Spring Cloud Config
    - 분산 설정은 마이크로서비스에서 필요한 설정 정보를 중앙에서 관리하고, 변경사항을 런타임 중에 적용할 수 있는 기능
    - 분산 환경에서의 설정 관리를 위한 `Spring Cloud Config`를 제공
2. 서비스 등록 및 검색(Service registration and discovery) : Spring Cloud Discovery
    -  `Netflix OSS`의 `Eureka, Apache ZooKeeper, Consul` 등 다양한 서비스 디스커버리 툴과 통합
3. 서비스 간 호출(Service-to-service calls) : Spring Cloud Discovery
4. 라우팅(Routing) : Spring Cloud Routing
5. 로드 밸런싱(Load balancing) : Spring Cloud Routing
    -  여러 개의 인스턴스에서 제공되는 서비스를 균등하게 분산시켜 주는 기능
    -  Spring Cloud는 Ribbon을 사용한 로드 밸런싱을 제공
6. 서킷 브레이커(Circuit Breakers/회로 차단) : Spring Cloud Circuit Breaker
    - 마이크로서비스에서 일시적으로 문제가 발생하여 서비스가 다운될 때, 다운된 서비스에 대한 요청을 차단하고 다른 대체 서비스를 호출하는 기능
    - Spring Cloud는 Netflix OSS의 Hystrix를 사용한 회로 차단기를 제공
7. 글로벌 락 (Global locks) & 지도자 선출, 클러스터 상태 (Leadership election and cluster state) : Spring Cloud Config
8. 분산 메시징 (Distributed messaging) : Spring Cloud Messaging
    

## Spring Cloud Config

1. Spring Cloud Config
    - 분산 시스템에서 외뷔화된 설정 정보를 서버 및 클라이언트에게 제공하는 시스템
    - 설정 서버(Config Server)는 외부에서 모든 환경에 대한 정보들을 관리해주는 중앙 서버
    - 본적으로 설정 정보 저장을 위해 git을 사용하도록 되어있어서 손쉽게 외부 도구들로 접근 가능하고, 버전 관리도 가능
    - Spring Cloud Config Server(설정 서버) : 버전 관리 레포지토리로 백업된 중앙 집중식 구성 노출을 지원
    - Spring Cloud Config Client(설정 클라이언트) : 애플리케이션이 설정 서버에 연결하도록 지원
    - 장/단점
        - 장점
            - 여러 서버의 설정 파일을 중앙 서버에서 관리
            - 서버를 재배포 하지 않고 설정 파일의 변경사항을 반영 가능
        - 단점
            - Git버서 또는 설정 서버에 의한 장애가 전파될 수 있다
            - 우선 순위에 의해 잘못된 설정 정보가 덮어씌워질 수 있다















