---
layout: post
title: SpringBoot File Upload하기 - [2]
---

내 프로젝트에 정리하여 업로드 완성시키기

## 프로젝트에 먼저 구현한 파일업로드 로직을 정리해서 적용시키기
- springboot 2.7.7 ver
- maven project
- JPA, myBatis 사용

1. controller
    ```java
    @RequestMapping("/updateInfo")
	public String memberUpdate(Model model,@RequestParam("password") String password,
			@RequestParam("email") String email, @RequestParam("phone") String phone,
			@RequestParam("city") String city, @RequestParam("country") String country,
			@RequestParam("introduce") String introduce,
			@RequestParam("userId") String userId, MultipartHttpServletRequest mtRequest) {
				
		// 이미지 처리
		MultipartFile mFile = mtRequest.getFile("img");
		// 이미지 저장 경로 설정
		String path = mtRequest.getServletContext().getRealPath("/resources/upload/profile/");
		File uploadPath = new File(path);
		// 디렉토리 생성
		if(!uploadPath.exists()) uploadPath.mkdirs();
        
		String oriFileName = "";
		String ext = "";
		String image = "";
		
		if(mFile != null) {
			if(!mFile.isEmpty()) {
				oriFileName = mFile.getOriginalFilename();
				ext = oriFileName.substring(oriFileName.lastIndexOf("."));
				image = userId+ext;
				
				try {
					mFile.transferTo(new File(path+image));
				}catch(IOException e) {
					System.out.println("실패");
				}
			}
			
		}
		
		Member m = Member.builder().userId(userId).city(city).country(country).email(email).password(password)
				.phone(phone).image(image).introduce(introduce).build();
		
		int result = service.updateMember(m);
		
		model.addAttribute("image", image);
		model.addAttribute("userId", userId);
		model.addAttribute("email", email);
		model.addAttribute("phone", phone);
		model.addAttribute("introduce", introduce);
		model.addAttribute("city", city);
		
		return "member/myPage1";
	}
    ```
    
테스트 프로젝트에서 구현한 로직에서 문제를 발견했다.<br>
파일 첨부를 안하는 경우에는 파일 저장을 하지 말아야 하는데 파일명과 함께 파일이 생성이 된다. <br>
<image src="https://user-images.githubusercontent.com/107177133/213079781-c121133e-0956-42f4-a51f-0a0603673f28.png"/>
- 해결 방법
    ```java
    if(mFile != null) {
        if(!mFile.isEmpty()) {
            oriFileName = mFile.getOriginalFilename();
            ext = oriFileName.substring(oriFileName.lastIndexOf("."));
            image = userId+ext;
            try {
                mFile.transferTo(new File(path+image));
            }catch(IOException e) {
                System.out.println("실패");
            }
        }
    }
    ```
    - 먼저 `if(mFile != null)`로 mFile이 null이 아닌 경우에만 파일 추출을 하도록 설정한다. 
        - null인 경우에는 첨부파일이 없는 것이다.
    - 그리고 `if(!mFile.isEmpty())`로 mFile의 문자열이 비어있지 않은지 체크한다
        - 생각해보니 굳이 이중조건문을 사용하지 않아도 되는것 같다..
    - 다음은 테스트 프로젝트에서 사용했듯이 파일 업로드 로직을 작성한다.
    - Update쿼리문이라 int형 변수로 받아주었다. -> update는 실패시 0을, 성공시 1을 반환.
    ```
    int result = service.updateMember(m);
	```
    - 앞서 parameter값으로 받아온 Model객체에 addAttributr()를 사용하여 데이터를 넣어준다.
        - Model은 HashMap 형태를 갖고 있으며, key, value값을 가지고 있다. 
        - 또한 addAttribute()와 같은 기능을 통해 모델에 원하는 속성과 그것에 대한 값을 주어 반환할 view에 데이터를 전달할 수 있다.

    
    ```java
	model.addAttribute("image", image);
	model.addAttribute("userId", userId);
	model.addAttribute("email", email);
	model.addAttribute("phone", phone);
	model.addAttribute("introduce", introduce);
	model.addAttribute("city", city);
    ```

-------
#### 번외
dao에서 jpa를 사용하지 않고 mybatis를 사용하였다.

- service에서 구현한 로직
	```java
	@Autowired
	private BCryptPasswordEncoder passwordEncoder;
	
	@Autowired
	private SqlSessionTemplate session;
	
	@Override
	public int updateMember(Member m) {
		m.setPassword(passwordEncoder.encode(m.getPassword()));
		// 회원가입 시 패스워드 암호화를 해주었기때문에 수정할때도 패스워드 암호화 설정을 해줘야 한다.
		return mmdao.updateMember(session, m);
	}
	```
	- Mybatis를 이용하여 DAO를 구현하려면 SqlSession 객체가 필요하다.
	- SqlSessionTemplate은 SqlSession을 구현하고 코드에서 SqlSession를 대체하는 역할이다.
	
- dao에서 구현한 로직
	```java
	public int updateMember(SqlSessionTemplate session, Member m) {
		return session.update("member.updateMember", m);
	}
	```
	- session객체의 메소드를 이용해서 쿼리문을 실행
		- DML : insert(), update(), delete() 
		- DQL : selectOne(), selectList()
		- 각 메소드의 매개변수로 mapper와 살행할 sql구문을 지정 
		- 문자열로 mapper의 namespace값.sql태그 아이디값
		- sql문을 실행할때 대입해야할 값이 있는 경우
		- sql처리 메소드의 두번째 매개변수에 값을 전달
		- (mybatis는 따로 다뤄보도록 하자.)\
		
	- `session.update("member.updateMember", m)`을 실행
		```java
		<mapper namespace="member">
			<update id="updateMember" parameterType="Member">
				UPDATE MEMBER SET 
				EMAIL=#{email}, PHONE=#{phone}, IMAGE=#{image}, CITY=#{city}, COUNTRY=#{country}, PASSWORD=#{password}, INTRODUCE=#{introduce} 
				WHERE USERID=#{userId}
			</update>
		</mapper>
		```
	- 이때 #{}를 사용하면 데이터를 문자열로 인식하고 자동으로 ""가 붙는다.
	- id값으로는 session.update("member.updateMember", m)에서 updateMember로 줘야하고,
	- 데이터를 전송받아서 처리하는 sql구문은 parameterType속성을 설정한다.

- 실행을 한게 되면 오류가 난다.
<image src="https://user-images.githubusercontent.com/107177133/213096517-0a853fc3-f38b-4547-9c3d-fff0c7ffa82c.png"/> <br>
	- 오류메세지를 보면 IllegalArgumentException 오류가 적혀있다.
	- 이 오류는
		- mapper XML파일에 작성한 id=''값과 DAO에 적은 id값이 다른 경우 
		- Parameter와 bean의 필드명이 틀린 경우
		- mapper XML파일에 정의된 네임스페이스(namespace)와 mapper파일에 직접 접근하는 java파일(DAO나 service)에서 호출하는 네임스페이스(namespace)가 다를 경우
		- MyBatis config파일에 mapper가 정의가 되어 있지 않거나 Spelling이 틀린 경우
		- mapper에 정의된 namespace 명칭이 같은 Application 내에 중복 될 경우
	- 전부 찾아보았다.
	- 찾아낸 오류는 application.yml에서 location설정을 하지 않았다.
		```java
		mybatis:
			mapper-locations: classpath:mappers/**/*.xml
			config-location: classpath:mybatis-config.xml
		```
		설정을 추가해 주고 실행을 하면 이미지 업로드가 잘 된다. <br>
		
------
	
### myBatis 설정
<image src="https://user-images.githubusercontent.com/107177133/213098932-7456998a-7c3f-4940-8c97-5acf23ddaeff.png"/> <br>
- window -> prefrences -> xml -> 사진에서 보이는 것처럼 설정을 추가해주었다.
- 굳이 안해도 되지만 안하게 되면 mapper-xml파일을 만들 때 
- `<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >`
- 를 작성해줘야 한다.
