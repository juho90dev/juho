---
layout: post
title: Markdown 사용법
---

마크다운 markdown 작성법
======================

# 1. 마크다운에 관하여
## 1.1. 마크다운이란?
[**Markdown**](https://www.markdownguide.org/getting-started/)은 텍스트 기반의 마크업언어로 2004년 존그루버에 의해 만들어졌으며 쉽게 쓰고 읽을 수 있으며 HTML로 변환이 가능하다. 특수기호와 문자를 이용한 매우 간단한 구조의 문법을 사용하여 웹에서도 보다 빠르게 컨텐츠를 작성하고 보다 직관적으로 인식할 수 있다.
마크다운이 최근 각광받기 시작한 이유는 깃헙([https://github.com](https://github.com)) 덕분이다. 깃헙의 저장소Repository에 관한 정보를 기록하는 README.md는 깃헙을 사용하는 사람이라면 누구나 가장 먼저 접하게 되는 마크다운 문서였다. 마크다운을 통해서 설치방법, 소스코드 설명, 이슈 등을 간단하게 기록하고 가독성을 높일 수 있다는 강점이 부각되면서 점점 여러 곳으로 퍼져가게 된다.

## 1.2. 마크다운의 장-단점

### 1.2.1. 장점
    1. 간결하다.
    2. 별도의 도구없이 작성이 가능하다.
    3. 다양한 형태로 변환이 가능하다.
    4. 텍스트(text)로 저장되기 때문에 용량이 적어 보관이 용이하다.
    5. 텍스트파일이기 때문에 버전관리시스템을 이용하여 변경이력을 관리할 수 있다
    6. 지원하는 프로그램과 플랫폼이 다양하다.
    
### 1.2.2. 단점
    1. 표준이 없다.
    2. 표준이 없기 때문에 도구에 따라서 변환방식이나 생성물이 다르다.
    3. 모든 HTML 마크업을 대신하지 못한다.


## 1. Headers 헤더
* `#`으로 시작하는 텍스트.
* `#`은 하나부터 여섯개까지 가능.
* `#`이 늘어날때마다 제목의 스케일 낮아집니다.
* H1은 `===`로도 만들 수 있습니다.
* H2는 `---`로도 만들 수 있습니다.

### Syntax 마크다운 사용법
    This is an H1
    ===
    This is an H2
    ---
    # This is an H1
    ## This is an H2
    ### This is an H3
    #### This is an H4
    ##### This is an H5
    ###### This is an H6   

### Demonstration 실행결과

이것은 H1<br>
===
This is an H2<br>
---
# 이것은 H1; 부(parts)에 사용<br>
## 이것은 H2; 장(chapters)에 사용<br>
### 이것은 H3; 페이지 섹션에 사용<br>
#### 이것은 H4; 하위 섹션에 사용<br>
##### 이것은 H5; 하위 섹션 아래의 하위 섹션에 사용<br>
###### 이것은 H6; 문단에 사용<br>


``` java
public class Array_lotto {
	public static void main(String[] args) {
		int lotto[] = new int [6];
		
     	  	// 번호 생성
		for(int i=0; i<6; i++) {
			lotto[i] = (int)(Math.random() * 45) + 1;
            
       		  	 // 중복 번호 제거
			for(int j=0; j<i; j++) {
				if(lotto[i] == lotto[j]) {
					i--;
					break;
				}
			}
		}
	System.out.print("로또 번호: ");
	
  	 // 번호 출력
	for(int i=0; i<6; i++) {
		System.out.print(lotto[i] + " ");
	}	
	}
}


```
