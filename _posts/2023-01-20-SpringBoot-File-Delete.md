---
layout: post
title: SpringBoot-File-Delete
---

## 업로드된 파일 삭제

### 이번에는 업로드된 파일을 삭제해 보자.
    1. 공지사항 글 삭제
    2. DB에 있는 데이터만 삭제하는 것이 아닌 저장경로에 있는 해당 첨부파일 또한 삭제해야 한다.

1. controller
    ```java
    @RequestMapping("/deleteNotice/{id}")
	public ModelAndView noticeDelete(@PathVariable int id,HttpServletRequest mtRequest, ModelAndView model ) {
		
		Notice n = service.selectNoticeInfo(id);
		
		String path  = mtRequest.getServletContext().getRealPath("/resources/upload/notice/");
		
		File f = new File(path+n.getRenameFileName());
		if(f.exists()) f.delete();
		
		int result = service.deleteNotice(id);
        
		if(result>0) {
			model.addObject("msg", "공지사항 삭제 성공");
			model.addObject("loc","/notice");
		}else {
			model.addObject("msg", "공지사항 삭제 실패");
			model.addObject("loc","/notice");
		}
		
		model.setViewName("common/msg");		
		return model;
	}
    ```
    
    - 정말 간단하다.
    - controller에서 `service.selectNoticeInfo(id);` 로직으로 해당 글의 정보를 가져왔다.
    - 그리고 파일의 저장경로를 가져온다.
    ```java
    File f = new File(path+n.getRenameFileName());
    ```
    - File객체에 집어 넣은 후
    ```java
    if(f.exists()) f.delete();
    ```
    - 처음에 `service.selectNoticeInfo(id);`로 해당 글의 데이터를 가져온 이유는
    - n.getRenameFileName()의 데이터값을 가져오기 위함이었다.
    - 그 다음 exists()를 사용한다.
        - exists() : 파일 또는 폴더가 존재하는지 리턴하는 메서드
    - 해당 경로에 존재한다면 delete()를 사용하여 삭제한다.

----------
### 파일 삭제 끝.
