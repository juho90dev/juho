---
layout: post
title: SpringBoot File Download하기 - [2]
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
    
    
