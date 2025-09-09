# 문서 객체 모델 (DOM)

--- 


# DOM(문서 객체 모델) 개요
- DOM은 HTML 문서의 구조를 트리(노드 계층) 형태로 표현한 인터페이스입니다. 브라우저는 HTML을 파싱해 DOM 트리를 만들고, 자바스크립트를 통해 이 트리를 읽고 수정할 수 있습니다.
- DOM은 문서의 구조(요소), 속성(attribute), 텍스트 등을 객체로 표현하므로 자바스크립트에서 이 객체들을 조작해 화면을 변경합니다.
- 최상위 타입은 Node이며, 그 하위에 Element, Text, Comment, Attr 등 구체적인 노드 타입이 있습니다.


![domexample.png](domexample.png)
-> 이 구조를 코드로 나타내면,
```html
<html>
	<head>
		<title>DomModle</title>
	</head>
	<body>
		<h1>Dom Flair's tutorial</h1>
		<p>DomTree</p>
		<p id = 'p1'>This is ..</p>
	</body>
</html>

```


# DOM 트리의 구성 요소(주요 노드 타입)
- Document: 전체 문서를 나타내는 루트 객체(document).
- Element: <div>, <p> 같은 HTML 요소. 속성(attributes)과 자식 노드를 가질 수 있음.
- Text: 요소 내부의 텍스트 내용을 표현하는 노드.
- Comment: 주석 노드.
- Attr: 요소의 속성(대부분 요즘은 Element.getAttribute/setAttribute로 다룸).

# 노드 취득 방법 (브라우저 환경에서 자주 쓰는 API)
- document.getElementById(id) — 단일 요소(Element). id는 문서 내에서 유니크해야 함.
- document.getElementsByTagName(tagName) — HTMLCollection(라이브 컬렉션).
- document.getElementsByClassName(className) — HTMLCollection(라이브).
- document.querySelector(selector) — 첫 매치 하나(Element).
- document.querySelectorAll(selector) — NodeList(정적 컬렉션).

예:
```javascript
// JavaScript
const el = document.getElementById('myId');
const items = document.querySelectorAll('.item');
const first = document.querySelector('ul > li');
```


# NodeList vs HTMLCollection 차이
- HTMLCollection: "라이브(live)" 컬렉션. DOM 변경이 컬렉션에 즉시 반영됩니다.
- NodeList: 보통 정적(특히 querySelectorAll로 얻은 경우) — 생성 시점의 스냅샷을 가집니다. 단, 일부 API(예: childNodes)는 라이브일 수도 있음.
- 실무 팁: 반복문에서 컬렉션을 변경(요소 추가/삭제)할 경우 라이브 컬렉션이면 인덱스 혼란이 날 수 있으므로 주의. 안전을 위해 Array.from(...)으로 배열로 변환해 사용.

# 노드 생성/삽입/삭제/교체 관련 API
- document.createElement(tagName) — 새 Element 생성.
- document.createTextNode(text) — 텍스트 노드 생성(요즘은 element.textContent로 간단히 처리).
- element.setAttribute(name, value) / element.getAttribute(name) / element.removeAttribute(name)
- parent.appendChild(node) — 마지막 자식으로 추가.
- parent.insertBefore(newNode, referenceNode) — referenceNode 앞에 삽입.
- parent.removeChild(node) — 자식 노드 삭제(요즘은 node.remove() 사용 가능).
- parent.replaceChild(newNode, oldNode) — 교체.
- node.cloneNode(deep) — 깊은 복사(자식까지 복사하면 deep=true).

예:
```javascript
// JavaScript
const p = document.createElement('p');
p.textContent = '안녕하세요';
p.setAttribute('class', 'greeting');
document.body.appendChild(p);
```


# 속성/콘텐츠 관련(자주 쓰는 프로퍼티)
- element.innerHTML — 요소 내부의 HTML 전체 읽기/쓰기. (주의: 보안(XSS) 및 성능)
- element.textContent — 텍스트만 읽기/쓰기(HTML 파싱 없음)
- element.classList — class 조작(추가/제거/토글/체크): add, remove, toggle, contains
- element.style — 인라인 스타일 접근(권장은 CSS 클래스 활용)

# 이벤트와 DOM
- addEventListener(type, handler, options) — 이벤트 리스너 등록(권장)
- removeEventListener — 제거
- 이벤트 객체(event): target, currentTarget, stopPropagation(), preventDefault() 등
- 이벤트 버블링/캡처링: DOM 트리를 타고 올라가거나 내려가는 특성
- 이벤트 위임(event delegation): 많은 자식 요소 각각에 이벤트를 걸지 않고, 공통 부모에 하나의 이벤트를 걸어 처리 — 동적 컨텐츠에도 유리하고 메모리 절약

예 (이벤트 위임):
```javascript
// JavaScript
const list = document.querySelector('#items');
list.addEventListener('click', (e) => {
  const li = e.target.closest('li');
  if (!li) return;
  console.log('Clicked item:', li.dataset.id);
});
```


# 성능과 최적화 팁
- 빈번한 DOM 조작을 할 때는 문서의 실제 재레이아웃(reflow) / 재페인트(repaint)가 많이 발생합니다. 이를 피하려면:
	- 변경을 모아서 한 번에 반영 (fragment 사용: DocumentFragment).
	- innerHTML 한번에 치환: 여러 번의 appendChild 대신 텍스트(HTML) 조합 후 innerHTML로 설정(단, 보안 주의).
	- display: none으로 숨겨 변경 후 다시 보이기 — 레이아웃 비용 감소에 도움될 때가 있음.
	- 읽기 작업(예: offsetWidth)와 쓰기 작업(예: style.left) 순서를 섞지 말고 그룹화(읽기 후 쓰기).
- 레이아웃 트래싱(layout thrashing)을 피하기: 반복문 내에서 getBoundingClientRect나 offsetHeight를 계속 조회하면 성능 저하가 심함.

# 보안 관련 주의
- innerHTML을 사용자 입력으로 직접 설정하면 XSS(크로스사이트스크립팅) 취약점이 생깁니다. 사용자 콘텐츠는 반드시 escape하거나 textContent를 사용하세요.
- 속성/데이터로 외부값을 주입할 때도 검증이 필요합니다.

# 접근성(Accessibility) 고려
- 동적으로 생성하는 콘텐츠는 스크린 리더가 인지하도록 ARIA 속성(aria-live, role 등)을 적절히 사용하세요.
- 포커스(예: focus()) 관리: 모달을 열 때 포커스를 적절히 설정하고 닫을 때 원래 포커스로 복원하세요.

# DOM 관련 실무 팁 & 권장 패턴
- 가능한 DOM 변경은 최소화하고, 로직은 DOM 변경 후에 실행하세요.
- 자주 사용하는 DOM 요소는 변수에 참조해 재조회 비용을 줄이세요.
- 템플릿 생성에는 템플릿 문자열(ES6 템플릿), <template> 태그, 혹은 클라이언트 템플릿 라이브러리 사용을 고려하세요.
- 복잡한 UI에서는 프레임워크(React/Vue/Svelte 등)의 가상 DOM/렌더 전략을 사용하면 직접 DOM 조작의 버그와 성능 관리를 줄일 수 있음.
- 이벤트 위임을 적극 활용하면 메모리와 코드량을 줄일 수 있음.

# 예제: 동적 아이템 추가(안전하고 효율적으로)
```javascript
// JavaScript
const ul = document.querySelector('#list');
function addItem(text) {
  const li = document.createElement('li');
  li.textContent = text; // 안전한 텍스트 설정
  ul.appendChild(li);
}
addItem('새 항목');
```


# 요약
- DOM은 HTML 문서를 자바스크립트로 조작할 수 있게 하는 중요한 인터페이스입니다.
- 노드 취득/생성/삽입/삭제 등 기본 API를 잘 이해하고, NodeList와 HTMLCollection의 특성(라이브 여부)을 주의하세요.
- 성능과 보안을 고려해 DOM 변경을 최소화하고, 사용자 입력을 직접 innerHTML에 넣지 마세요.
- 이벤트 위임과 document fragment 같은 기법을 활용하면 더 효율적이고 유지보수성 높은 코드를 작성할 수 있습니다.

원하시면:
- 위 내용을 README 형태(마크다운 파일)로 정리해 드리거나,
- 예제 파일(간단한 데모 HTML/JS)을 만들어 드리겠습니다.
  원하시는 형식을 알려주세요.
