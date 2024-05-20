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
6. 서킷 브레이커(Circuit Breakers) : Spring Cloud Circuit Breaker
7. 글로벌 락 (Global locks) & 지도자 선출, 클러스터 상태 (Leadership election and cluster state) : Spring Cloud Config
8. 분산 메시징 (Distributed messaging) : Spring Cloud Messaging
    
