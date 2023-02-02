---
layout: post
title: SpringBoot-공공API-활용하기
---

## SpringBoot-공공API-활용하기

- springboot 2.7.7 ver
- maven project
- JPA사용
- 공공API(OPEN API) - 한국관광공사_국문 관광정보 서비스


1. 프로젝트를 진행하면서 필요한 공공API(OPEN API)를 활용하기.
    - https://www.data.go.kr/에서 필요한 공공데이터를 검색<br>
        <image src="https://user-images.githubusercontent.com/107177133/216039645-a614b261-640c-4054-923b-f4ea33ed4a37.png" width=60%/>
    - 사용할 API를 선택 <br>
        <image src="https://user-images.githubusercontent.com/107177133/216042534-9f8b39f9-9198-496e-a655-48bb8dd3e672.png" width=60%/>
    - 활용신청 버튼을 클릭 <br>
    - 클릭 후 활용목적과 저작권표시를 동의 한 후 활용 신청을 한다.
    - 승인이 되고 해당 데이터를 상세보기를 하면 <br>
        <image src="https://user-images.githubusercontent.com/107177133/216220605-55d2b782-9342-4299-9610-c9da046a9d3f.png" width=60%/>
    - 위와 같이 인증키를 발급받은 것을 볼 수가 있다. 위 사진의 인증키는 보안의 이유로 지운 상태이다.
    - 일반 인증키 - api를 호출할 때 데이터가 파라미터로 들어가게 된다.
    - EndPoint - 호출 url이다.
        - 이 두가지와 Api에서 정해놓은 파라미터들을 활용해 데이터를 가져올 수 있는 것이다.
2. 그럼 불러올 경우에 어떻게 나오는지 확인해 보자.
    - 예시 url
        - https://apis.data.go.kr/B551011/KorService/areaBasedList?serviceKey=[인증키]&pageNo=1&numOfRows=1&MobileApp=AppTest&MobileOS=ETC&arrange=A&areaCode=1
        - `https://apis.data.go.kr/B551011/KorService` 여기까지 endpoint
        - `areaBasedList` 는 해당 데이터에서 제공하는 서비스중 하나이며 지역기반서비스다.
        - `areaCode=1` 지역코드가 1인 데이터로 설정
    - XML 형식 <br>
        <image src="https://user-images.githubusercontent.com/107177133/216222785-f1a1ddc0-5600-4c6a-a7b5-4398894acdc2.png" width=60%/>
    - JSON 형식 <br>
        <image src="https://user-images.githubusercontent.com/107177133/216222990-6863da6e-66b9-40ad-bfa5-36a9fccca2ba.png" width=60%/>
    
