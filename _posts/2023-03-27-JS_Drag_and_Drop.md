---
layout: post
title: Drag And Drop 하기
---

## JavaScript Drag And Drop
### 자바스크립트를 이용해 예제를 구현해보자.

HTML 그리고 JavaScript에서의 드래그 앱 드롭은 이벤트 기반으로 작동하게 되며, 마우스 커서로 객체(object)를 드래그해서 놓을 때까지 여러 단계의 이벤트가 순차적으로 발생하게 된다.
먼저 드래그 & 드롭 이벤트 종류에 대해 알아보도록 하자.

1. 드래그 & 드롭 이벤트 종류
    - dragstart	: 사용자가 객체(object)를 드래그하려고 시작할 때 발생함.
    - drag : 대상 객체를 드래그하면서 마우스를 움직일 때 발생함.
    - dragenter : 마우스가 대상 객체의 위로 처음 진입할 때 발생함.
    - dragover : 드래그하면서 마우스가 대상 객체의 영역 위에 자리 잡고 있을 때 발생함.
    - drop : 드래그가 끝나서 드래그하던 객체를 놓는 장소에 위치한 객체에서 발생함. 리스너는 드래그된 데이터를 가져와서 드롭 위치에 놓는 역할을 함
    - dragleave	: 드래그가 끝나서 마우스가 대상 객체의 위에서 벗어날 때 발생함.
    - dragend : 대상 객체를 드래그하다가 마우스 버튼을 놓는 순간 발생함.
    - 물론 전부 사용할 필요는 없고 필요한 것만 사용해도 된다. 하지만이중 drop, dragover 이벤트는 필수로 사용해야 한다. dragover 이벤트를 적용하지 않으면 drop 이벤트가 작동하지 않는다..
* 드래그 할때는 드래그 이벤트를 적용할 요소에서 draggable=true를 설정해야 드래그가 가능하다.
```html
<div class="player" draggable="true">
</div>
```

2. 내가 구현한 드래그앱드롭
    - 먼저 html을 보자.
        ```html
        <div class="players">
            <div class="player" draggable="true">
                <li id="li1" class="item">
                    <img src="https://img.khan.co.kr/news/2020/09/09/2020091001001134700093591.jpg" class="playerImg" width="110px" height="86px">
                    <div class="info">
                        <h5 class="name">케빈 더 브라위너</h5>
                        <span class="team">맨체스터 시티</span>
                    </div>
                </li>
            </div>
            <div class="player" draggable="true">
                <li id="li1" class="item">
                    <img src="https://www.mancity.com/meta/media/nthb2hil/haaland-signs.jpg?width=560&height=315" class="playerImg" width="110px" height="86px">
                    <div class="info">
                        <h5 class="name">엘링 홀란드</h5>
                        <span class="team">맨체스터 시티</span>
                    </div>
                </li>
            </div>
            <div class="player" draggable="true">
                <li id="li1" class="item">
                    <img src="https://blog.kakaocdn.net/dn/RsGww/btrlM2jMgoo/sBB8t5VkKwWuRy9cBH9H9k/img.jpg" class="playerImg" width="110px" height="86px">
                    <div class="info">
                        <h5 class="name">필 포든</h5>
                        <span class="team">맨체스터 시티</span>
                    </div>
                </li>
            </div>
        </div>
        ```
    - 해당 HTML을 웹에서 출력하면 이렇게 나온다(적용된 css는 일단 빼고 작성)
        <br>
        <img src="https://user-images.githubusercontent.com/107177133/229477160-47ca00b8-7dfc-4689-80fe-e433108716aa.png" width="300px" height="300px"/>
        <br>
    - 그럼 JavaScript를 보자
        ```javascript
        // 드래그 앤 드롭 
            var dragSrcEl = null;
            function dragStart(e) {
                this.style.opacity = '0.4'; // 드래그할때 투명도가 40%로 떨어짐.
                dragSrcEl = this;
                
                e.dataTransfer.effectAllowed = 'move';
                e.dataTransfer.setData('text/html', this.innerHTML);
            }
            
            function dragOver(e) {
                if (e.preventDefault) {
                e.preventDefault();
                }
                e.dataTransfer.dropEffect = 'move';
                return false;
            }
            function handleDrop(e) {
                if (e.stopPropagation) {
                    e.stopPropagation(); // e.stopPropagation는 이벤트가 상위 엘리먼트에 전달되지 않게 막아 준다.
                }
                if (dragSrcEl != this) { // 드래그한 열과 드롭할 열이 동일한지 확인합니다.
                    dragSrcEl.innerHTML = this.innerHTML;
                    this.innerHTML = e.dataTransfer.getData('text/html');
                    //
                }
            return false;
            }

            function dragEnd(e) {
                this.style.opacity = '1'; // 드랍할때 투명도 다시 100%로 올라감.
            
                items.forEach(function (item) {
                    item.classList.remove('over');
                });
            }

            let items = document.querySelectorAll('.players .player');
            items.forEach(function(item) {
                item.addEventListener('dragstart', dragStart);
                item.addEventListener('dragover', dragOver);
                item.addEventListener('drop', handleDrop);
                item.addEventListener('dragend', dragEnd);
            });
        ```
        
     - 드래그를 시작할때 불투명도를 적용했다. 물론 필수적으로 적용할 필요는 없는데 드래그 하고있다는 div라는걸 표시하기 위해 적용했다.
     - 드래그가 끝나면 다시 불투명도는 100%로 오도록 설정
        ```javascript
        function handleDragStart(e) {
            this.style.opacity = '0.4';
        }
        function handleDragEnd(e) {
            this.style.opacity = '1';
        }
        ```
    - 드래그할 경우 해당 링크로 이동하는 브라우저의 기본 동작을 방지해야 한다. 그래서 dragover 이벤트에서 e.preventDefault()를 호출한다.
    - preventDefault()
        - 메서드는 어떤 이벤트를 명시적으로 처리하지 않은 경우, 해당 이벤트에 대한 사용자 에이전트의 기본 동작을 실행하지 않도록 지정하는 것
    <br>
    
    ```javascript
    function dragOver(e) {
        if (e.preventDefault) {
            e.preventDefault();
        }
        e.dataTransfer.dropEffect = 'move';
        return false;
    }
    ```            
    - 드래그 앤 드롭시 자료를 전달하기 위해서는 dataTransfer 객체를 사용해야 한다. 
        - dataTransfer 객체는 이벤트의 속성으로 드래그한 항목에서 타겟의 정보를 전송할 수 있다.
        - 나는 setData(), getData() 두개의 메소드를 사용했다.
        
        ```javascript
        // drag를 시작할때
        e.dataTransfer.setData('text/html', this.innerHTML);
        
        // 드랍을 할때(드래그이벤트가 끝나는 시점)
        this.innerHTML = e.dataTransfer.getData('text/html');
        ```
    
    - 마지막으로 드래그이벤트를 적용할 div를 불러온다.
    - 그리고 해당 div에 각각의 이벤틀르 적용시켜준다.
        ```javascript
        let items = document.querySelectorAll('.players .player');
        
        items.forEach(function(item) {
            item.addEventListener('dragstart', dragStart);
            item.addEventListener('dragover', dragOver);
            item.addEventListener('drop', handleDrop);
            item.addEventListener('dragend', dragEnd);
        });
        ```














