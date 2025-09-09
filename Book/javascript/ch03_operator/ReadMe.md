## CH03_Operator
- 산술 연산자: +, -, *, /, %, **(거듭제곱)
- 할당 연산자: =, +=, -=, *=, /=, %=, **=
- 비교 연산자:
	- == (느슨한 동등, 타입 강제 변환 후 비교) — 예외와 혼동의 원인이므로 사용에 주의
	- === (엄격한 동등, 타입까지 비교) — 권장
	- !=, !==, >, >=, <, <=
- 논리 연산자: &&, ||, ! (단축 평가(short-circuit) 동작)
- 삼항 연산자: condition ? exprIfTrue : exprIfFalse
- 비트 연산자: &, |, ^, ~, <<, >>, >>>
- null 병합 연산자: ?? (왼쪽 값이 null 또는 undefined면 오른쪽 반환)
- 옵셔널 체이닝: obj?.prop (안전하게 접근)

예제 (연산자)


```javascript
// JavaScript
const x = 5;
const y = '5';

console.log(x == y);  // true  (타입 강제 변환)
console.log(x === y); // false (타입이 다름)

const obj = { a: { b: 1 } };
console.log(obj?.a?.b); // 1
console.log(null ?? 'default'); // 'default'
```
----
