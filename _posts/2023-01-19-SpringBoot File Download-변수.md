---
layout: post
title: SpringBoot File Download하기
---

## File Download를 하자.


### File Upload를 구현했으니 이번 post에서는 Download를 구현해보자.
1. controller
    ```java
    public void fileDownload(String oriname, String rename, HttpServletResponse res, 
                HttpServletRequest req, @RequestHeader(value="User-agent") String header) {
		
		String path = req.getServletContext().getRealPath("/resources/upload/notice/");
		File saveFile = new File(path+rename);	
        
		try(BufferedInputStream bis = new BufferedInputStream (new FileInputStream(saveFile));
            ServletOutputStream sos = res.getOutputStream();){

			boolean isMS = header.contains("Trident")||header.contains("MSIE");
			
			String encodeFilename = "";
			if(isMS) {
				encodeFilename = URLEncoder.encode(rename, "UTF-8");
				encodeFilename = encodeFilename.replaceAll("\\+", "%20");
						}else {
				encodeFilename = new String(oriname.getBytes("UTF-8"),"ISO-8859-1");
			}
			
			res.setContentType("application/octet-stream;charset=utf-8");
			res.setHeader("Content-Disposition", "attachment;filename=\""+encodeFilename+"\"");
						
			int read=-1;
			while((read=bis.read())!=-1) {
				sos.write(read);
			}
			
		}catch(IOException e) {
			e.printStackTrace();
		}
		
	}
       
    ```
    - 응답하기 위해 HttpServletResponse를 받아왔다
        - HttpServletResponse객체에 Content Type, 응답코드, 응답 메시지등을 담아서 전송하기 위해서다.
    - 경로를 받아오기 위해 HttpServletRequest를 받아왔다.
		- 인코딩처리 : @RequestHeader(value="User-agent")
    - 다운받을 파일의 위치를 가져온다.
		```java
		String path = req.getServletContext().getRealPath("/resources/upload/notice/");
		File saveFile = new File(path+rename);	

		```
	- BufferedInputStream  : byte단위로 파일을 읽어 올때 사용하는 버퍼 스트림이다.
		- 직접 코드를 읽어 파일에 접속 하는 것보다 버퍼를 이용해서 속도를 증가 시킨다.
			```java
			BufferedInputStream bis = new BufferedInputStream (new FileInputStream(saveFile))은 

			FileInputStream fis = new FileInputStream(saveFile);
			BufferedInputStream bis = new BufferedInputStream(fis)를 한번에 쓴것이다.
			```
		- ServletOutputStream - 이미지와 같이 바이너리 데이터를 웹 브라우저에 전달하기 위해서 사용.
			```java
			ServletOutputStream sos = res.getOutputStream();
			```
			- 즉 InputStream으로 File을 읽어와서 OutputSteram으로 파일을 내보낸다.
	- 인코딩하기
		```java
		boolean isMS = header.contains("Trident")||header.contains("MSIE");
		```
		-  User-Agent header 값을 가져와서 브라우저 별로 인코딩을 해 줘야 한글이 깨지지 않고 정상 출력이 된다.
		-  IE11부터는 MSIE가 없고 Trident로 변경되었기때문에 "Trident"를 찾거나 "MSIE"를 찾는 로직을 작성해야 한다. <br>
		```java
		String encodeFilename = "";
			if(isMS) {
				encodeFilename = URLEncoder.encode(rename, "UTF-8");
				encodeFilename = encodeFilename.replaceAll("\\+", "%20");
			}else {
				encodeFilename = new String(oriname.getBytes("UTF-8"),"ISO-8859-1");
			}
		```
		- replace() 함수는 대상 문자열을 원하는 문자 값으로 변환하는 함수다.
		- URL 인코딩은 일반적으로 공백을 더하기 (+) 기호 또는 %20으로 변경한다고 한다.
		- `URLEncoder.encode`를 하면 공백부분에 +가 생긴다. 
		- 그렇기 때문에 replaceAll로 +를 공백 인코딩문자(%20) 으로 바꿔준다. 
		- replaceAll을 안하면 공백에 +가 보인다.
		- `getBytes()` 메서드는 String(유니코드 문자열)을 charset으로 인코딩하여 byte 배열로 반환해준다.

		 
		```java
		- res.setContentType("application/octet-stream;charset=utf-8");
		```
		- setContentType 메소드는 html의 표준 MIME 타입인 "text/html" 의 변경이나 인코딩을 재 지정하고자 할때 사용한다.
		- application/octet-stream의 경우 8비트 바이너리 배열인데
		- http나 이메일상에서 application이 지정되지 않거나 형식을 모를 때 사용한다.
		
		```java
		res.setHeader("Content-Disposition", "attachment;filename=\""+encodeFilename+"\"");
		```
		- Content-Disposition은 HTTP Response Body에 오는 컨텐츠의 기질/성향을 알려주는 속성이다.
			- 이때 `"attachment;filename=\""`를 주게 되면 Body에 오는 값을 다운로드받으라는 뜻이 된다.
		- attachment : 로컬에 다운로드 & 저장 (대부분의 브라우저에서는 바로 다운로드가 되거나, “Save As” 다이얼로그가 표시됨)
		- filename : 파일 이름을 지정할 수 있는데
			- encodeFilename을 적어서 다운로드 받을때 인코딩한 데이터를 파일이름으로 한다는 뜻이다.
	- 읽어온 파일을 출력하기
		- read() : byte 단위로 읽어오며 파일을 읽어올때 성공하면 수신바이트 수, 실패시 -1을 반환한다.
			- 파일 다 읽으면 read()는 -1반환한다는 말이다.
		```java
		int read=-1;
		while((read=bis.read())!=-1) {
			sos.write(read);
		}
		=> (read=bis.read()) != -1이 아니면 
			=> 읽어온 값을 write로 출력한다.
		```


--------

1. 파일 다운로드 <br>
	<image src="https://user-images.githubusercontent.com/107177133/213630426-57505986-051b-4004-808e-4d1202cbb64e.png" style="width:400px; height:200px;"/> <br>
	<br>
2. 클릭<br>
	<image src="https://user-images.githubusercontent.com/107177133/213630659-0d96324d-e16d-4412-949f-22202a8d79c7.png" style="width:400px; height:200px;"/> <br>
																													


