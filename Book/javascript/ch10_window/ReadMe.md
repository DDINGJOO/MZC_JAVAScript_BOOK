# Window 내장객체 

---

- 개요
  - 브라우저 탑 레벨 객체로 타이머, 다이얼로그, 위치, 화면, 히스토리 등 하위 API의 진입점입니다.

- 관련 실습 파일
  - window_01.html: window.confirm 다이얼로그 사용
  - window_02.html: setTimeout 기본 동작
  - window_03.html: setInterval 및 clearInterval
  - window_04.html: 시계 예제(Interval 기반)
  - window_05.html: 재귀 setTimeout 기반 타이머

- 주요 API 요약
  - 다이얼로그: alert, confirm, prompt
  - 타이머: setTimeout/clearTimeout, setInterval/clearInterval
  - 전역: window, document, location, navigator, history, screen

- 팁
  - setInterval 대신 재귀 setTimeout을 사용하면 지연 누적에 대응하기 쉽습니다.
  - 타이머 ID를 저장해 두고 해제하는 습관을 들이세요.

