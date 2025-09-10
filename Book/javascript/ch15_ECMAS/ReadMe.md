# ECMA Script(ECMAScript) 정리 노트
---

목차
1. 역사와 배경
2. 모듈(ESM)과 스코프
3. 변수 선언 (var / let / const) 및 호이스팅
4. 함수: 선언/표현식/화살표 함수, arguments, 기본 파라미터, rest
5. this 바인딩과 call/apply/bind
6. 클로저와 렉시컬 스코프
7. 객체 리터럴 개선(계산된 프로퍼티, 축약 프로퍼티 등)
8. 프로토타입과 클래스 (class, extends, super)
9. 배열과 이터러블, 스프레드/전개, 구조 분해 할당
10. 비동기 처리: Promise, async/await, 이벤트 루프
11. 모던 문법: Optional Chaining, Nullish coalescing, BigInt, Symbol 등
12. 고급 주제: 모듈 번들링/트랜스파일링, 폴리필, 성능/메모리 고려사항
13. 실습 팁과 체크리스트
14. 자주하는 실수와 디버깅 요령
15. 참고자료 및 학습 로드맵

---

1. 역사와 배경
- ECMAScript(ES)는 JavaScript의 표준 명세입니다. 주요 버전:
	- ES5(2009): strict mode, JSON, Function.prototype.bind 등
	- ES6 / ES2015: let/const, 화살표 함수, 클래스, 모듈, 템플릿 리터럴, 디스트럭처링, 프로미스, 심볼, Map/Set 등 혁신적 기능 다수
	- 이후 매년 ES2016~ES2024로 점진적 개선 (예: async/await, optional chaining, BigInt 등)
- 브라우저 호환성 이슈 때문에 Babel/TypeScript와 폴리필(core-js 등)을 통해 트랜스파일링/폴리필 적용이 일반적입니다.
- 실무: 최신 문법을 사용하되, 타겟 브라우저/런타임 환경(browserslist)에 맞춰 빌드 파이프라인을 설계하세요.


2. 모듈(ESM)과 스코프
- ESM (import/export)
	- 정적 분석 가능: 번들러가 트리 쉐이킹(사용하지 않는 코드 제거) 가능
	- export default와 named export 차이
	- 동적 import(import('path'))는 Promise 반환하며 코드 분할(코드 스플리팅)에 유용
- 스크립트 전역 vs 모듈:
	- 모듈은 기본적으로 strict mode이고 각 파일마다 자체 스코프를 가집니다.
- CommonJS (require/module.exports)와의 차이: 동기 로드 vs ESM의 정적 바인딩 및 비동기 로드 특성
- 실무 팁: 라이브러리는 ESM 번들과 CommonJS 번들 모두 제공하면 호환성에 유리


3. 변수 선언 (var / let / const) 및 호이스팅
- var
	- 함수 레벨 스코프(함수 내부에선 전역처럼 동작), 선언이 호이스팅되어 undefined로 초기화
	- 재선언 허용 — 의도치 않은 버그의 원인
- let / const
	- 블록 레벨 스코프, Temporal Dead Zone(TDZ) 존재: 선언 전 접근 시 ReferenceError
	- const는 재할당 불가(원시값에 대해 불변), 객체의 내부 프로퍼티는 변경 가능(얕은 불변)
- 권장 패턴
	- 가능한 const를 기본으로 사용, 재할당 필요 시 let 사용, var 사용 금지
	- 상수 이름은 관례적으로 ALL_CAPS (프로젝트 규칙에 따름)
- 예시(오류 유발 패턴)
	- for (var i = 0; i < n; i++) { setTimeout(() => console.log(i), 0); } // 클로저와 var 때문에 모두 n 출력
	- 대신 let i 사용하면 의도대로 동작


4. 함수: 선언/표현식/화살표 함수, arguments, 기본 파라미터, rest
- 함수 선언문과 표현식의 차이(호이스팅)
	- 선언문은 전체가 호이스팅되어 선언 이전에 호출 가능
	- 표현식(변수에 할당된 함수)은 변수 호이스팅 규칙을 따름
- 화살표 함수(=>)
	- 간결 문법, 렉시컬 this 바인딩(화살표 함수 내부의 this는 상위 스코프의 this를 가리킴)
	- prototype 없음, 생성자(new)로 사용할 수 없음
- arguments
	- 함수 내부에서 유사배열 객체로 접근 가능(레거시)
	- ES6 이후엔 rest 파라미터(...args)를 사용 권장 (진짜 배열)
- 기본 파라미터
	- function f(a = 1, b = someExp()) {}
	- 기본값 표현식은 호출 시점에 평가
- rest 파라미터와 스프레드 연산자 차이
	- Rest: 선언부에서 여러 인자를 배열로 수집 (function f(...args) {})
	- Spread: 호출/리터럴에서 배열/이터러블을 펼침 (f(...arr), [...arr])
- 주의: 화살표 함수는 arguments 바인딩이 없음(필요하면 rest 사용)

5. this 바인딩과 call/apply/bind
- this 결정 규칙(우선순위)
	1. new 바운드(생성자 호출) — 새 객체
	2. call/apply/bind 명시적 바인딩
	3. 암시적 바인딩(obj.method() 호출)
	4. 기본 바인딩 (전역 객체 또는 undefined(strict mode))
- 화살표 함수는 렉시컬 this — 호출 문맥에 따라 달라지지 않음
- call/apply/bind
	- call(thisArg, arg1, arg2...), apply(thisArg, [args...]), bind(thisArg) -> 새로운 함수 반환(영구적 바인딩)
- 실무 권장: 클래스 메서드에서 this 문제를 피하려면 화살표 필드(프로퍼티)나 constructor에서 bind 사용

6. 클로저와 렉시컬 스코프
- 클로저: 내부 함수가 외부 함수의 변수에 접근 가능하고 외부 함수의 실행 컨텍스트가 종료되어도 참조가 유지되는 현상
- 용도: 정보 은닉(모듈화), 부분 적용, 팩토리 함수, 상태 유지
- 메모리 주의: 불필요하게 큰 데이터를 클로저로 캡처하면 GC가 해제하지 못해 메모리 누수 발생 가능
- 패턴: 필요한 것만 캡처, 타임아웃/이벤트 리스너 해제

7. 객체 리터럴 개선(계산된 프로퍼티, 축약 프로퍼티 등)
- 축약 프로퍼티: { x, y }는 { x: x, y: y } 와 동일
- 계산된 프로퍼티: {[expr]: value}
	- 동일 키로 중복 선언 시 뒤의 선언이 덮어씀(재할당)
- 메서드 축약: { foo() { ... } } 는 function foo() {} 의 축약형
- 객체 복사/병합: Object.assign({}, obj) 또는 스프레드 {...obj}
	- 주의: 얕은 복사(shallow copy). 중첩 객체는 그대로 참조
- 심화: 불변성 패턴에서 스프레드로 새로운 객체를 만들어 업데이트 (예: React state 업데이트)

8. 프로토타입과 클래스 (class, extends, super)
- 클래스는 문법적 설탕(syntactic sugar)이며 내부적으로 프로토타입 기반 상속을 사용
- class Foo { constructor() {} method() {} static s() {} }
	- static은 인스턴스가 아닌 클래스에 바인딩
- 상속: class Child extends Parent { constructor(...){ super(...); } }
- super 키워드: 부모의 생성자 호출 또는 메서드 접근
- prototype 체인과 instanceof, __proto__ 차이 이해 중요
- 메서드 바인딩: 클래스 메서드 내부의 this는 런타임 호출 방식에 따라 달라짐(필요 시 bind 혹은 화살표 필드 사용)

9. 배열과 이터러블, 스프레드/전개, 구조 분해 할당
- 이터러블: 배열, 문자열, Map, Set, arguments 등
- for...of 루프는 이터러블을 순회, for...in은 열거 가능한 프로퍼티 키를 열거(객체에서 사용 시 의도치 않은 키 포함 가능)
- 스프레드: [...iterable], 배열 병합/복제, 함수 호출 시 인수 펼치기
- 구조 분해 할당(Destructuring)
	- 배열: const [a, b = 2, ...rest] = arr;
	- 객체: const { x, y: aliasY = 1, ...rest } = obj;
	- 중첩 구조 분해와 기본값 조합 가능
- 유용한 배열 메서드 (불변 패턴에 적합)
	- map, filter, reduce, some, every, find, findIndex, flat, flatMap
- 성능 팁
	- 빈번한 대규모 배열 복사는 비용이 큼 — 필요 시 가변 구조로 작업한 뒤 불변화로 마무리
	- for 루프가 가장 빠르지만, 가독성과 의도 표현을 위해 고수준 메서드 사용 권장

10. 비동기 처리: Promise, async/await, 이벤트 루프
- Promise: new Promise((resolve, reject) => {})
	- then 체인: promise.then(...).catch(...).finally(...)
	- 주의: 체인을 반환하지 않으면 에러가 누락될 수 있음
- async/await
	- async function f() { const x = await p; }
	- await는 Promise로 변환 가능한 모든 값을 기다림
	- try/catch로 에러 처리 필요
- 이벤트 루프 이해
	- 마이크로태스크 큐(Promise 콜백)와 매크로태스크 큐(setTimeout 등)의 우선순위 차이
	- 예: Promise.resolve().then(...)은 setTimeout보다 먼저 실행
- 병렬/동시 실행 패턴
	- Promise.all, Promise.allSettled, Promise.race, Promise.any
	- 대량 요청 제어: concurrency 제한 라이브러리(p-limit 등)
- 실무 권장: 네트워크/IO는 비동기 처리, UI 렌더링 블로킹 방지

11. 모던 문법: Optional Chaining, Nullish coalescing, BigInt, Symbol 등
- Optional chaining (?.)
	- 안전한 중첩 속성 접근: obj?.a?.b
	- 함수 호출에도 사용 가능: obj?.method?.()
- Nullish coalescing (??)
	- 왼쪽 값이 null 또는 undefined인 경우에만 대체값 사용: a ?? default
	- falsey(0, '', false)는 허용되는 값으로 취급
- BigInt
	- 큰 정수 지원: 123n
	- Number와 바로 혼합 불가 (명시적 변환 필요)
- Symbol
	- 유일한 식별자 생성에 사용, 객체의 숨겨진 속성 키로 유용
- 기타: Optional catch binding, dynamic import, top-level await(지원 환경 확인 필요)

12. 고급 주제: 모듈 번들링/트랜스파일링, 폴리필, 성능/메모리 고려사항
- 번들러(bundler)와 트랜스파일러
	- Babel: 최신 문법을 구형 환경으로 트랜스파일
	- TypeScript: 타입체크 + 트랜스파일
	- 번들러(webpack, rollup, parcel, vite 등): 코드 스플리팅, 트리 쉐이킹, 자원 번들링
- 폴리필
	- core-js 등으로 특정 API(예: Promise, Symbol, Array.from 등)를 폴리필
	- 폴리필은 런타임 크기와 충돌 위험 고려
- 빌드 설정 팁
	- browserslist를 통한 타겟 환경 설정
	- polyfills: entry 기준 or usage 기반 주입 선택
- 성능/메모리
	- 불필요한 전역 변수/싱글톤 과다 사용 주의
	- 이벤트 리스너/타이머 해제 누락으로 인한 메모리 누수 예방
	- 대용량 데이터 처리: 스트리밍 처리, Web Worker 분리
	- 렌더링 성능: 레이아웃 쓰레시, 리플로우 최소화 (문서 조작은 배치)

13. 실습 팁과 체크리스트
- 로컬 실습 환경 구성
	- 브라우저 콘솔 + Node.js REPL로 빠른 실험
	- Visual Studio Code + ESLint + Prettier 설정으로 일관된 스타일 유지
- 학습 체크리스트(중요 개념)
	- let/const/var와 TDZ 이해
	- 화살표 함수와 this의 차이
	- Promise 체인과 async/await 패턴
	- 구조 분해 할당과 스프레드/레스트
	- 모듈(import/export)과 번들링 기본
- 작은 연습 과제
	- 간단한 Todo 앱에서 상태 업데이트(불변 패턴) 구현
	- 비동기 파일 읽기/쓰기 시나리오를 Promise/async로 작성
	- 클래스 상속을 사용한 간단한 상속 구조 구현

14. 자주하는 실수와 디버깅 요령
- 화살표 함수에서 this 기대와 다른 경우
	- 디버깅: 콘솔에 this 출력하여 상위 스코프와 비교
- 비동기 코드에서 에러 누락
	- Promise 내부에서 throw -> then 체인에서 처리되지 않으면 UnhandledPromiseRejection
	- 항상 catch 또는 try/catch로 에러 핸들링
- 불변성 미준수로 인한 상태 버그
	- 원시값은 괜찮지만 객체/배열은 깊은 복사를 고려하거나 불변 라이브러리 사용
- 성능 문제 디버깅
	- 크롬 DevTools 성능 탭 사용, 프로파일링으로 레이아웃/스크립트 병목 확인
- 모듈 로드 문제
	- ESM과 CommonJS 혼용 시 default export/ named export 혼동 주의

15. 실무 예제/패턴 모음 (요약)
- 안전한 디폴트 맵 초기화
	- const map = new Map(); map.set(key, (map.get(key) || 0) + 1);
- 불변 업데이트(객체)
	- const next = { ...state, nested: { ...state.nested, prop: newVal } };
- 함수형 유틸(부분 적용, 커링)
	- const add = x => y => x + y;
- debounce/throttle: 입력 이벤트 최적화
	- lodash.debounce 사용 또는 직접 구현

---

심화 설명: 몇 가지 핵심 항목 상세해설

A. 이벤트 루프와 마이크로태스크/매크로태스크
- 호출 스택(Call Stack): 실행 중인 함수 컨텍스트
- 태스크 큐(Task Queue)
	- 매크로태스크(macrotasks): setTimeout, setInterval, I/O callbacks, UI rendering 등
	- 마이크로태스크(microtasks): Promise callbacks, queueMicrotask 등
- 한 이벤트 루프 사이클에서
	1. 콜 스택이 비면 마이크로태스크 큐가 처리됨(모든 마이크로태스크가 소진될 때까지)
	2. 그 후 매크로태스크 하나가 실행
- 결과: Promise.then(...) 콜백은 setTimeout(...,0)보다 먼저 실행됨
- 학습 팁: 복잡한 비동기 버그는 이 우선순위 때문에 발생하므로 작은 예제(콘솔 로그 시퀀스)를 통해 직접 확인

B. 얕은 복사 vs 깊은 복사
- 얕은 복사
	- Object.assign({}, obj) 또는 {...obj} — 1계층만 복사, 속성 값이 참조 타입이면 동일 참조 유지
- 깊은 복사
	- JSON.parse(JSON.stringify(obj)): 간단하지만 함수, undefined, Symbol, 순환 참조 처리 불가
	- 라이브러리 사용: lodash.cloneDeep, structuredClone (환경 지원 시)
- 실무 방안
	- 복잡한 상태 업데이트에는 불변성 helper(immer) 사용하면 복잡도를 크게 줄일 수 있음

C. 모듈(트리 쉐이킹과 사이드 이펙트)
- 트리 쉐이킹은 정적 분석으로 사용하지 않는 export를 제거
- 사이드 이펙트가 있는 모듈(모듈 수준에서 실행되는 코드)이 있으면 트리 쉐이킹이 제한될 수 있음
- 권장: 모듈은 가능한 한 순수하게(사이드 이펙트 최소화) 작성


---

부록: 빠른 문법 요약(핵심 코드 스니펫)
- 변수
	- const PI = 3.14;
	- let count = 0;
- 화살표 함수와 rest
	- const sum = (...nums) => nums.reduce((a,b)=>a+b,0);
- 구조 분해
	- const [first, ...rest] = arr;
	- const { a, b: alias = 2 } = obj;
- Promise/async
	- async function fetchJSON(url) { const res = await fetch(url); return res.json(); }
- 스프레드
	- const arr2 = [...arr1, ...arr3];
	- const obj2 = { ...obj1, newProp: 1 };

---

