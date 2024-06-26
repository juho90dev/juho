---
layout: post
title: SpringBoot File Upload하기 - [1]
---


## SpringBoot로 파일 업로드[1]

- `SpringBoot` 2.7.7버전을 사용
- `maven project`
- test패키지를 만들어서 테스트 로직 구현


1. 회원 프로필 사진을 바꾸기 위해 이미지파일을 form태그를 이용하여 전송한다.
    ```java
    <form action="/upload" method="post" enctype="multipart/form-data">
    ```
    이때 `enctype`을 주목하자.
    - `<form enctype="속성값>`
    - 3가지 속성이 있다.
        1. application/x-www-form-urlencoded : 기본값으로, 모든 문자들은 서버로 보내기 전에 인코딩됨을 명시한다.
        2. multipart/form-data : 모든 문자를 인코딩하지 않음을 , 이 방식은 form 요소가 파일이나 이미지를 서버로 전송할 때 주로 사용한다.
        3. text/plain : 공백 문자(space)는 “+” 기호로 변환하지만, 나머지 문자는 모두 인코딩되지 않음을 명시한다.
  
 
---------

2. controller  
    - controller에서 구현한 로직은 다음과 같다.
    
        ```java
        @RequestMapping("/upload")
        public String upload(MultipartHttpServletRequest mtRequest, 
        @RequestParam("writer") String writer, @RequestParam("content") String content,Model m) {
        
        MultipartFile mFile = mtRequest.getFile("image");
        
		String path = mtRequest.getServletContext().getRealPath("/resources/upload/test/");
		File uploadPath = new File(path);
		
		if(!uploadPath.exists()) uploadPath.mkdirs();
		
		// String oriFileName = mFile.getOriginalFilename();
		// String ext = oriFileName.substring(oriFileName.lastIndexOf("."));
		
		String oriFileName = "";
		String ext = "";
		if(mFile != null) {
			try {
				oriFileName = mFile.getOriginalFilename();
				ext = oriFileName.substring(oriFileName.lastIndexOf("."));
				
			}catch (Exception e) {
				System.out.println("등록 실패");
			}
		}
				
		String rename = writer+ext;
		try {
			mFile.transferTo(new File(path+rename));
		}catch(IOException e) {
			System.out.println("실패");
		}
		
		
		Upfile f = Upfile.builder().writer(writer).content(content)
				.originalFileName(oriFileName).renameFileName(rename).build();
		// Upfile f1 = Upfile.builder().writer(writer).content(content).build();
	
		
		us.insertFile(f);
		
		
		m.addAttribute("rename",rename);
		m.addAttribute("writer", writer);
		m.addAttribute("content", content);
		m.addAttribute("oriFileName", oriFileName);
		
		return "mypage";
	    }
        ```


---------

3.  getFile()
	```java
    MultipartFile mFile = mtRequest.getFile("image");
    getFile()을 이용해서 MultipartFile에 넘어온 이미지주소를 넣는다.
    ```    


---------

4. 그리고 이미지 저장 경로를 설정
	```java
    String path = mtRequest.getServletContext().getRealPath("/resources/upload/test/");
    File uploadPath = new File(path);
	```
	- File클래스로 File 생성
	- 클래스를 생성하는 건 RAM상에서 생성하는 것이기 때문에 
	- 실제 HDD에 생성하는 것은 아니다.
	- 실제 HDD에 파일로 생성하려면 `createNewFile()`메소드를 이용 
	- new File(String path)은 주어진 문자열 경로를 갖는 File 객체를 생성한다
    - 절대 경로 또는 현재 프로그램이 실행되는 위치를 기준으로 하는 상대경로로 지정 가능하다.<br>
    	내가 설정한 경로를 넣고 uploadPath로 설정    


---------

5. 저장 경로명에 해당되는 디렉터리가 존재하지 않을 경우, 해당 디렉터리가 자동으로 생성되도록 설정
	- `mkdirs()`를 사용.
	```java
	if(!uploadPath.exists()) uploadPath.mkdirs();
	```
	-  여기서 잠깐! <br>
       	경로`.exists()` : 경로에 file/directory(folder)가 존재하는지 확인한다. <br>
        경로`.isFile()` : 경로가 file인지 확인한다. <br>
        경로`.isDirectory()` : 경로가 directory(folder)인지 확인한다. <br>
		<br>	
		
---------

6. 그리고 파일 이름과 확장자를 추출한다.
	```java
	String oriFileName = mFile.getOriginalFilename();
	String ext = oriFileName.substring(oriFileName.lastIndexOf("."));
	```
	- 하지만 여기서 주의할 점! <br>
		oriFileName.lastIndexOf(".") 이 부분에서 "." 뒤 인덱스를 찾고 있기 때문인데 첨부 파일이 없을 경우에는 인덱스를 찾는것이 불가능하기 때문이다.<br>
		lastIndexOf(검색할 값 , 시작위치)는 끝에서부터 검색할 값을 찾기 시작한다.<br>
		인수를 찾을 수 없는 경우 -1을 반환한다.<br>
	- 아래에 있는 `오류메시지`를 보자<br>
	<br>
	<image src="https://user-images.githubusercontent.com/107177133/212882308-9a530a0b-f89d-468e-9d8f-04f25a8146bd.png"/>
	<br>
	- substring(oriFileName.lastIndexOf(".")) 부분에서 오류가 난다 -> 조건문을 사용해서 mFile이 null이 아닌 가정을 두고 try~catch문으로 작성한다.

---------

7. 저장될 이름을 작성한 뒤 `transferTo()`로 복사한다.
	```java
	String rename = writer+ext;
		try {
			mFile.transferTo(new File(path+rename));
		}catch(IOException e) {
			System.out.println("실패");
		}
	```

---------

8. 그 뒤 Upfile클래스에 builder()를 사용하여 데이터를 넣어주고 DB에 넣는 메서드를 사용한다.
	```java
	Upfile f = Upfile.builder().writer(writer).content(content).originalFileName(oriFileName).renameFileName(rename).build();
	
	us.insertFile(f);
	```
	
--------

9. 구현화면
	- 사진을 선택 후 업로드를 하면<br>
		<image src="https://user-images.githubusercontent.com/107177133/213077036-7821df9e-0779-4e3b-a13d-a4e999782e77.png"/> <br>
	- 해당 디렉토리에<br>
		<image src="https://user-images.githubusercontent.com/107177133/213076133-3a460ea7-2f55-4caa-a39f-e4a6acd931f7.png" width="250" height="150"/><br>
	- 업로드한 파일이 내가 설정한 이름의 이미지파일이 저장된 것을 확인 할 수 있다.<br>
		<image src="https://user-images.githubusercontent.com/107177133/213076608-bcce7316-728c-448d-a33c-4aeb60dc418a.png" width="250" height="150"/>
