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
        <image src="https://user-images.githubusercontent.com/107177133/216039645-a614b261-640c-4054-923b-f4ea33ed4a37.png" width=600 height=200/>
    - 사용할 API를 선택 <br>
        <image src="https://user-images.githubusercontent.com/107177133/216042534-9f8b39f9-9198-496e-a655-48bb8dd3e672.png" width=600 height=200/>
    - 활용신청 버튼을 클릭 <br>
    - 클릭 후 활용목적과 저작권표시를 동의 한 후 활용 신청을 한다.
    - 승인이 되고 해당 데이터를 상세보기를 하면 <br>
        <image src="https://user-images.githubusercontent.com/107177133/216220605-55d2b782-9342-4299-9610-c9da046a9d3f.png" width=600 height=200/>
    - 위와 같이 인증키를 발급받은 것을 볼 수가 있다. 위 사진의 인증키는 보안의 이유로 지운 상태이다.
    - 일반 인증키 - api를 호출할 때 데이터가 파라미터로 들어가게 된다.
    - EndPoint - 호출 url이다.
        - 이 두가지와 Api에서 정해놓은 파라미터들을 활용해 데이터를 가져올 수 있는 것이다.
        - 
2. 그럼 불러올 경우에 어떻게 나오는지 확인해 보자.
    - 예시 url
        - https://apis.data.go.kr/B551011/KorService/areaBasedList?serviceKey=[인증키]&pageNo=1&numOfRows=1&MobileApp=AppTest&MobileOS=ETC&arrange=A&areaCode=1
        - `https://apis.data.go.kr/B551011/KorService` 여기까지 endpoint
        - `areaBasedList` 는 해당 데이터에서 제공하는 서비스중 하나이며 지역기반서비스다.
        - `areaCode=1` 지역코드가 1인 데이터로 설정
    - XML 형식 <br>
        <image src="https://user-images.githubusercontent.com/107177133/216222785-f1a1ddc0-5600-4c6a-a7b5-4398894acdc2.png" width=600 height=200/>
    - JSON 형식 <br>
        <image src="https://user-images.githubusercontent.com/107177133/216224637-b76acb45-80f6-4cb6-83cb-7edba322587d.png" />
    - 해당 url에 `&_type=json`을 작성하면 JSON형식으로 응답이 오고 작성을 하지 않을 경우에는 XML형식으로 응답이 온다.

3. 데이터를 사용하기 위해 controller를 생성후 데이터를 불러와보자.
    - controller 작성(service 및 repository는 일반 데이터 저장과 다를바가 없어 생략)
    
    ```java
    @RestController
    public class apiTestController {
	
	@Autowired
	private ApiService as;
	
	@GetMapping("/test")
	public String testBasic() throws IOException{
		StringBuilder result = new StringBuilder();
		String urlStr = "http://apis.data.go.kr/B551011/KorService/areaBasedList?"
				+ "serviceKey=[서비스 인증키]"
				+ "&pageNo=2"
				+ "&numOfRows=100"
				+ "&MobileApp=AppTest"
				+ "&MobileOS=ETC"
				+ "&_type=json"
				+ "&areaCode=1";


            URL url = new URL(urlStr);

            HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();
            urlConnection.setRequestMethod("GET");
            BufferedReader br;
            br = new BufferedReader(new InputStreamReader(urlConnection.getInputStream(), "UTF-8"));
            String returnLine;

            while((returnLine = br.readLine()) != null) {
                result.append(returnLine +"\n\r");

            }

            urlConnection.disconnect();
            String jsonData = result.toString();

            try {

                JSONObject jObj;
                JSONParser jsonParser = new JSONParser();
                JSONObject jsonObj=(JSONObject) jsonParser.parse(jsonData);

                JSONObject parseResponse = (JSONObject) jsonObj.get("response");

                JSONObject parseBody = (JSONObject) parseResponse.get("body");

                JSONObject parseItems = (JSONObject) parseBody.get("items");

                JSONArray array = (JSONArray) parseItems.get("item");


                for(int i = 0; i<array.size(); i++) {
                    jObj=(JSONObject)array.get(i);
                    TestVo testvo = TestVo.builder().address(jObj.get("addr1").toString())
  						.areacode(Integer.parseInt(jObj.get("areacode").toString()))
						.sigungucode(Integer.parseInt(jObj.get("sigungucode").toString()))
                        .cat1(jObj.get("cat1").toString())
                        .cat2(jObj.get("cat2").toString())
                        .contentId(jObj.get("contentid").toString())
                        .contentTypeId(jObj.get("contenttypeid").toString())
                        .firstImage(jObj.get("firstimage").toString())
                        .mapx(jObj.get("mapx").toString())
                        .mapy(jObj.get("mapy").toString())
                        .title(jObj.get("title").toString())
                        .build();

                    as.insertData(testvo);
               }
            }catch(Exception e) {
                e.printStackTrace();
            }
            return result.toString();
        }
    }

    ```
    
    - url에 Open API의 EndPoint를 넣어주고 사용할 서비스명을 적어준다. 
    - 그리고 끝부분에 `?`를 적는다.
    - serviceKey=[] : 작성을 하지 않아 보이지는 않지만 이 부분에 마찬가지로 신청한 Api의 serviceKey를 적어준다.
    - pageNo= 표시할 페이지번호
    - numOfRows=표시할 데이터의 수
    -` _type=json` : 응답 표준은 XML 이기에, JSON을 요청할경우 `&_type=json`을 작성을 해줘야 JSOM 방식으로 응답이 온다.
    - `https://apis.data.go.kr/B551011/KorService/areaBasedList?` -> https://를 사용했더니 오류가 나기에 찾아보니 Java의 신뢰하는 인증서 목록(keystore)에 사용하고자 하는 인증기         관이 등록되어 있지 않아 접근이 차단되는 현상이라고 한다.
    - 그 말은 즉, Https://를 호출하기 위해서는 Java에 인증서 목록에 해당 기관을 등록을 해줘야 한다는 것이데 등록이 되지않았다면 오류가 난다는 소리다.
    -  인증서를 추가와 인증서 목록에 추가하여도 접근이 안되는 경우가 있는 경우는 https 대신 http로 작성하는 방법이다
    
    - URLConnection의 클래스는 일방적인 URL에 대한 API를 제공하고, 서브클래스 HttpURLConnection는 HTTP 고유의 기능에 대한 추가 지원을 제공한다.
	다만 이 두 클래스는 모두 추상클래스이기에 새 인스턴스를 직접 만들수가 없다.
	대신 URL객체에서 연결을 통해 URLConnection의 인스턴스를 얻는 방법이 있다.
	먼저 URL객체를 생성한다.
    ```java
    URL url = new URL(urlStr);
    ```
    - url(urlString)으로 url을 생성
    	- URLConnection 인스턴스는 URL 객체의 openConnection() 메소드 호출에 의하여 얻어진다.
    	- 프로토콜이 Http:// 인 경우 반환된 객체를 HttpURLConnection객체로 캐스팅 할수 있다고 한다.
    ```java
    HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();
    ```
    - Http 메소드 중 하나인 URL요청에 대한 메소드를 설정, 기본값은 GET
    ```java
    urlConnection.setRequestMethod("GET");
    ```
    - 실제 내용을 읽기 위해 InputStream 인스턴스를 얻은 다음 read()를 이용하여 데이터를 읽어야한다.
	나는 한줄씩 읽기위해 readLine을 사용
	데이터를 result에 추가하고 읽을 데이터가 null이 된다면(없다면) 반복문을 해제하도록 while문 사용
	그리고 연결해제
    ```java
    BufferedReader br;
		br = new BufferedReader(new InputStreamReader(urlConnection.getInputStream(), "UTF-8"));
		String returnLine;
		
		while((returnLine = br.readLine()) != null) {
			result.append(returnLine +"\n\r");
			
		}
		urlConnection.disconnect();
	```
	- close()를 사용해셔 연결을 종료한 경우 다시 연결할려면 openConnection() 을 다시 해줘야 하고, disconnect()는 connect() 메소드만 호출하면 바로 다시 복구 된다. 
	- 여기까지가 아까 예시로 웹상에서 보았던 데이터를 볼 수가 있다.
4. JSON Parsing
	- JSON이란?
		- JSON은 `J`ava`S`cript `O`bject `N`otation의 약자다.
		- JSON은 "네트워크를 통해 데이터를 주고받는 데 자주 사용되는 경량의 데이터 형식"이다.  
		- JSON은  name - value 형태의 쌍으로 이루어져있다.
		- JSON은 독립적으로 다양한 언어에 사용
		- JSON 오브젝트는 XML 데이타가 타입이 없는데 비해 타입을 가진다.
			- XML 데이타는 모두 String이다.
			- JSON types : string, number, array, boolean
	- Parsing이란?
		- 다른 형식으로 저장된 데이터를 원하는 형식의 데이터로 변환하는 것을 말한다.
			-  XML, JSON : 데이터를 표현하는 문자열
			-  JSON 파싱 : JSON 형식의 문자열을 객체로 변환하는 것
	
	- 그렇다면 이제 JSON-parsing을 해보자.
	- 하지만 여기서 잠깐!!!
	- JSON을 사용하기 위해 Json library가 필요하다.
	- pom.xml에 가서 dependency를 추가하자.
	```java
	<!-- JSON-->
	<dependency>
	    <groupId>com.googlecode.json-simple</groupId>
	    <artifactId>json-simple</artifactId>
	    <version>1.1.1</version>
	</dependency>
	```
	- result에 담겨진 데이터를 parsing하기 위해 새로운 객체에 담아준다.
	```java
	String jsonData = result.toString();
    ```
	- 잘몬된 Json데이터가 들어와서 에러발생이 되는 것을 방지하기 위해 try ~ catch문으로 시작을 하고.
	- 먼저 JSON 객체를 생성하고 JSON 파싱 객체 생성한다.
	```java
	JSONObject jObj;
	JSONParser jsonParser = new JSONParser();
	```
	- 파싱할 String Data(StringBuilder result)를 JSON객체로 parser를 통해 저장한다.
	```java
	JSONObject jsonObj=(JSONObject) jsonParser.parse(jsonData);
	```
	- 데이터를 이제 나눠보자.
	- response, body, items, item를 받아온다.
	- parserResponse에는 response내부의 데이터가 담겨있다.
	```java
	JSONObject parseResponse = (JSONObject) jsonObj.get("response");
	JSONObject parseBody = (JSONObject) parseResponse.get("body");
	JSONObject parseItems = (JSONObject) parseBody.get("items");
	```
	
	- 이때 item 내부의 데이터는 [] 형태 -> 배열형태이니 Json배열로 받아온다.
	
	```java
	JSONArray array = (JSONArray) parseItems.get("item");
   	 ```
	 
	- 이제 DB에 저장하는 로직이다.
	- 데이터가 여러개이니(지역코드가 1인 서울의 관광데이터만해서 7천개가 넘는다....) 반복문을 사용해서 넣을 것이다.
	
	```java
	for(int i = 0; i<array.size(); i++) {
		jObj=(JSONObject)array.get(i);
		TestVo testvo = TestVo.builder().address(jObj.get("addr1").toString())
			.areacode(Integer.parseInt(jObj.get("areacode").toString()))
			.sigungucode(Integer.parseInt(jObj.get("sigungucode").toString()))
			.cat1(jObj.get("cat1").toString())
			.cat2(jObj.get("cat2").toString())
			.contentId(jObj.get("contentid").toString())
			.contentTypeId(jObj.get("contenttypeid").toString())
			.firstImage(jObj.get("firstimage").toString())
			.mapx(jObj.get("mapx").toString())
			.mapy(jObj.get("mapy").toString())
			.title(jObj.get("title").toString())
			.build();
		as.insertData(testvo);
				
				
	}
	```
	- url작성에서부터 필요한 데이터만 선택 후 불러왔기때문에 해당 데이터에 맞는 객체를 담은 클래스를 작성했다.
	- 이 작업에서는 테스트로 했기 때문에 TextVo로 VO클래스를 작성했다.
