# 이벤트(Event)

---

- 개요
  - 사용자 입력(클릭, 키보드, 포커스 등)과 시스템 변화(로드, 네트워크 등)에 반응하는 메커니즘.
  - 이벤트 객체(e)는 종류(type), 대상(target), 현재 리스너가 붙은 요소(currentTarget) 등의 정보를 담음.

- 이벤트 등록 방식
  - 인라인: onclick="..." (Event_01.html)
  - 프로퍼티: window.onload = fn (Event_03.html)
  - 표준 리스너: addEventListener(type, listener, useCapture) (Event_04.html, Event_05.html, ...)

- this와 화살표 함수
  - 일반 함수 핸들러에서 this는 이벤트 대상 요소 (Event_05.html)
  - 화살표 함수는 상위 스코프의 this를 사용하여 요소가 아님 (Event_06.html)

- 이벤트 전파(버블링/캡처링)
  - 기본은 버블링: 안쪽 → 바깥쪽으로 전파 (Event_09.html) , 캡쳐 : 바깥 -> 안쪽으로 전파 ,true
  - 전파 중단: e.stopPropagation() (Event_09.html 주석 참고)

- 이벤트 위임(Delegation)
  - 상위 요소에 리스너를 등록하고 하위에서 발생한 이벤트를 판별 처리 (Event_10.html, Event_11.html)
  - e.target으로 실제 발생 요소를 확인, e.currentTarget은 리스너가 등록된 요소

- 기본 동작 제어
  - 링크/폼 등 기본 동작 취소: e.preventDefault() (Event_12.html, Event_13.html, Event14.html, Event_14.html)

- 리스너 관리
  - addEventListener / removeEventListener (Event_07.html)

- 실습 맵
  - Event_01.html ~ Event_02.html: 인라인, 이벤트 객체 기본
  - Event_03.html ~ Event_06.html: 로드 이벤트, this vs 화살표
  - Event_07.html ~ Event_11.html: 리스너 제거, 전파, 위임
  - Event_12.html ~ Event_14.html, Event14.html: 기본 동작 제어와 폼 검증

- 예시 코드
```html
<button id="btn" type="button">Click</button>
<script>
  const btn = document.getElementById('btn');
  btn.addEventListener('click', function(e){
    console.log(this === e.currentTarget); // true
  });
</script>
```
