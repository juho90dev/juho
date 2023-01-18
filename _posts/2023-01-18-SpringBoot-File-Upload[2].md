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




