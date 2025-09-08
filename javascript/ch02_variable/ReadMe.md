

## CH02_Variable
- 변수 선언 방법 및 타입
	- 자바스크립트는 동적 타이핑을 사용합니다(변수 선언 시 타입을 명시하지 않음).
	- 기본 타입(원시 타입, primitive)
		- number (정수/부동소수 구분 없음, IEEE-754 double)
		- string
		- boolean (falsy 값: 0, '', null, undefined, NaN, -0 등)
		- undefined
		- null
		- symbol (ES6)
		- bigint (ES2020, 큰 정수 처리)
	- 참조 타입(객체)
		- object, array, function, class 등



- 변수 선언 키워드와 유효 범위
	- var
		- 함수 레벨 스코프(function scope)
		- 호이스팅 시 선언과 동시에 undefined로 초기화되어 선언 이전에도 접근 가능(의도하지 않은 동작 유의)
	- let
		- 블록 레벨 스코프(block scope)
		- 선언 이전에는 접근 불가 (TDZ: Temporal Dead Zone)
	- const
		- 블록 레벨 스코프
		- 선언과 동시에 초기화 필요, 재할당 불가(단 객체의 내부 프로퍼티는 변경 가능)


- Hoisting(호이스팅)
	- 식별자(변수/함수)의 선언 정보가 실행 컨텍스트의 맨 위로 끌어올려지는 현상
	- 함수 선언문은 전체가 호이스팅 되어 선언 이전에 호출 가능
	- var는 선언 시 자동 초기화되어 참조 가능하지만, let/const는 선언은 끌어올려지지만 초기화 전 접근 시 ReferenceError 발생 (TDZ)


- 타입 확인
	- typeof 연산자: console.log(typeof value)
	- 배열과 null 구분: Array.isArray(value) / null은 typeof가 "object" 이므로 주의


예제 (var/let/const와 스코프)
```javascript
// JavaScript
var a = 1;
let b = 2;
const c = 3;

console.log(a, b, c); // 1 2 3

{
var a_inBlock = 10;
let b_inBlock = 20;
const c_inBlock = 30;
console.log(a_inBlock, b_inBlock, c_inBlock); // 10 20 30

console.log(a, b, c); // 1 2 3
}

console.log(a_inBlock);            // 10   (var는 함수/전역 스코프)
console.log(typeof b_inBlock);     // "undefined" (b_inBlock은 블록 스코프라 전역에서 접근 불가)
```
----
