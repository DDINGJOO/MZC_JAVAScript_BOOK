
## CH04_Function
- 함수 개요
	- 함수는 특정 작업을 수행하는 독립적인 코드 블록입니다.
	- 자바스크립트에서 함수는 일급 객체(First-class citizen)입니다:
		- 함수를 변수에 할당할 수 있다.
		- 함수를 매개변수로 전달할 수 있다.
		- 함수를 반환값으로 반환할 수 있다.



- 함수의 종류 및 예제
	- 함수 선언문 (Function Declaration)


```javascript
function sum(a, b) {
return a + b;
}
console.log(sum(2, 3)); // 5
```


- 함수 표현식 (Function Expression) / 익명 함수


```javascript
const multiply = function(a, b) {
return a * b;
};
console.log(multiply(2, 3)); // 6
```



- 화살표 함수 (Arrow Function)
```javascript
const add = (a, b) => a + b;
console.log(add(2, 3)); // 5
```



- 즉시 실행 함수(IIFE: Immediately Invoked Function Expression)
```javascript
const result = (function(x) {
return x * 2;
})(5);
console.log(result); // 10
```


- 중첩 함수(Nested Function)와 클로저(Closure)
	- 중첩 함수는 함수 내부에 정의된 함수를 말하며, 외부 함수의 스코프를 참조할 수 있습니다.
	- 클로저: 내부 함수가 외부 함수의 렉시컬 환경(변수 등)을 기억하고 참조하는 성질입니다. 이를 활용해 데이터 은닉(캡슐화)을 구현할 수 있습니다.



예제 (클로저)


```javascript
function outerFunction() {
let x = 10;
function innerFunction(number) {
x += number;
return x;
}
return innerFunction;
}

const inner = outerFunction();
console.log(inner(5)); // 15
console.log(inner(2)); // 17
```
- 이벤트 핸들러와 클로저 활용 예 (브라우저)

```html
<button id="btn" type="button">Click</button>

<script>
  const btn = document.getElementById('btn');

  // 클로저 형태로 count를 은닉
  const increase = (function() {
    let count = 1;
    return function() {
      console.log('count =', count++);
    };
  })();

  btn.addEventListener('click', function() {
    increase();
  });
</script>
```


- 콜백함수(Callback Function)
	- 다른함수 (A)의 인자로 콜백함수(B)를 전달하면 A가 B의 제어권을 갖게 된다.
		- 제어권의 위임
			1. 어떤 시점에 콜백함수를 호출할 지
			2. 인자에 어떤 값들을 지정할지
			3. this에 무엇을 바인딩할 지

```javascript
function add(a, b, callback) {
	return callback(a, b);
}

function printConsole(result) {
	console.log(result);
}

add(1,2, printConsole);
```
