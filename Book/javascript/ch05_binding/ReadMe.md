
# BINDING (this 바인딩) 

## 1. 바인딩(this)의 필요성
- this는 "함수가 실행될 때 그 함수가 참조해야 할 객체(실행 컨텍스트)"를 가리킵니다.
- 동일한 함수가 여러 객체에서 재사용될 때 어떤 객체의 상태(프로퍼티)에 접근할지 결정하기 위해 필요합니다.
- 이벤트, 콜백, 비동기 코드 등 호출 시점이 달라지는 환경에서 함수 내부에서 올바른 객체에 접근하도록 보장합니다.

## 2. 바인딩 규칙(우선순위)
우선순위가 높은 쪽이 적용됩니다.
1. new (생성자 호출) — 새로 생성된 인스턴스가 this
2. 명시적 바인딩 — call / apply / bind로 지정한 객체
3. 암시적(메서드) 바인딩 — 객체.메서드() 호출 시 그 객체
4. 기본(전역/함수) 호출 — 일반 함수 호출: non-strict에서는 전역 객체(window/global), strict에서는 undefined
5. 화살표 함수(특별 규칙) — 위 규칙들을 무시하고 정의된 상위 스코프의 this(렉시컬 this)를 사용

특이사항: bind로 묶은 함수에 new를 사용하면 new가 우선으로 동작하는 등 일부 예외가 존재합니다.

## 3. 엄격모드(strict mode) 영향
- non-strict: 독립 함수 호출 시 this는 전역 객체 (브라우저: window, Node: global)
- strict: 독립 함수 호출 시 this는 undefined → 의도치 않은 전역 접근 방지

## 4. 핵심 예제

예: 전역/함수/메서드/생성자
```javascript
// JavaScript
function globalFunc() {
  console.log(this); // non-strict: window(global) / strict: undefined
}
globalFunc();

const obj = {
  name: 'obj',
  say() { console.log(this.name); } // 메서드 호출: obj가 this
};
obj.say();

function Person(name) { this.name = name; }
const p = new Person('Alice'); // 생성자 호출: p가 this
console.log(p.name); // 'Alice'
```


예: call, apply, bind
```javascript
// JavaScript
function greet(greeting, punctuation) {
  console.log(greeting + ', ' + this.name + punctuation);
}
const person = { name: 'Bob' };

greet.call(person, 'Hello', '!');   // Hello, Bob!
greet.apply(person, ['Hi', '...']); // Hi, Bob...
const boundGreet = greet.bind(person, 'Hey');
boundGreet('?'); // Hey, Bob?
```


예: 화살표 함수(렉시컬 this)
```javascript
// JavaScript
const module = {
  id: 42,
  regular: function() { console.log(this.id); }, // 42
  arrow: () => { console.log(this.id); } // 상위 스코프의 this 사용
};
module.regular();
module.arrow();
```


## 5. 콜백에서 자주 발생하는 문제 & 해결법
문제: 콜백이 독립적으로 호출되어 원래의 this를 잃음.

해결책 (상황별 권장)
1. 화살표 함수 사용 — 가장 간단. 상위 스코프의 this를 그대로 사용.
2. bind로 미리 this 고정 — 명확하고 안전하지만 bind는 새 함수를 반환.
3. 변수로 this 저장 (예: const self = this) — 레거시 환경에서 유용.
4. call/apply로 즉시 호출 — 호출 시점에 this를 지정할 때.

예:
```javascript
// JavaScript
const obj = {
  value: 10,
  runLater() {
    setTimeout(function() { console.log(this.value); }, 0); // undefined
  },
  runLaterFixed1() {
    setTimeout(() => { console.log(this.value); }, 0); // 10 (arrow)
  },
  runLaterFixed2() {
    setTimeout(function() { console.log(this.value); }.bind(this), 0); // 10 (bind)
  }
};
```


## 6. 활용 사례
- UI 이벤트 핸들러(컴포넌트 인스턴스의 상태 접근)
- 라이브러리/유틸 함수 재사용(call/apply로 다른 객체에 동작 적용)
- 콜백/이벤트 전달 시 context 보존(예: bind 또는 arrow)
- 클래스 내부에서 메서드를 this 바인딩해서 사용(React 클래스 컴포넌트 등)

## 7. 주의사항 / 팁
- bind는 새로운 함수(복사)를 반환하므로 반복적으로 bind하면 메모리/성능 고려 필요.
- 메서드를 다른 객체에 할당해 호출하면 this가 달라짐(의도치 않을 경우 주의).
- 화살표 함수는 this를 바인딩하지 않으므로 객체의 메서드로 정의 시 주의(메서드는 일반 함수로 선언 권장).
- strict 모드를 사용하여 의도치 않은 전역 this 접근을 방지하라.
- 라이브러리 API 설계 시 콜백에 this를 기대하지 않거나 명확히 문서화할 것(대신 필요한 context를 인자로 전달).

## 8. 디자인·코드 스타일 권장
- 함수형 스타일을 지향하면 this 의존을 줄일 수 있다(명시적 인자 전달).
- 클래스 내부에서 this가 필요한 메서드는 생성자에서 한 번만 bind하거나(메모리 고려) 클래스 필드에 화살표 함수를 사용해 바인딩할 수 있다.
- 콜백을 받을 때는 문서화로 기대하는 this 규약을 명확히 하라(또는 관련 객체를 인자로 넘겨라).

## 9. 디버깅 팁
- console.log(this)로 현재 컨텍스트 확인.
- 브라우저 디버거에서 호출 스택(call stack)과 스코프(scope) 살펴보기.
- 함수가 어디서 호출되는지(암시적, 명시적, new 등) 확인.
- bind 사용 시 새 함수가 반환되는 점(동일성 비교 실패)을 기억.

## 10. 요약 (한눈에)
- this는 호출 방식에 따라 동적으로 결정된다.
- 우선순위: new > call/apply/bind > 암시적(메서드) > 기본(전역/함수). 단, 화살표 함수는 렉시컬 this로 예외.
- 콜백 환경에서 주의가 필요하며 arrow / bind / self 패턴으로 해결.
- 설계 차원에서 this 의존을 줄이면 유지보수성과 확장성이 좋아진다.


## 11. 추가 실습 코드들 


1) 메서드 빌려쓰기 (method borrowing)
```javascript
// JavaScript
const alice = { name: 'Alice', greet() { console.log(`Hi, I'm ${this.name}`); } };
const bob = { name: 'Bob' };

// 메서드 빌려쓰기 — 암시적 바인딩을 통해 bob이 this가 됨
alice.greet();            // Hi, I'm Alice
alice.greet.call(bob);    // Hi, I'm Bob
```


2) apply vs call 차이 (인자 전달 방식)
```javascript
// JavaScript
function sum(a, b, c) { return a + b + c; }
console.log(sum.call(null, 1, 2, 3));    // 6
console.log(sum.apply(null, [1, 2, 3])); // 6
```


3) bind로 부분 적용(partial application) + 재사용
```javascript
// JavaScript
function multiply(a, b) { return a * b; }
const double = multiply.bind(null, 2); // 첫 인자 고정 -> 부분 적용
console.log(double(5)); // 10
```


4) bind한 함수와 new의 상호작용 (주의점)
```javascript
// JavaScript
function Person(name) { this.name = name; }
const Bound = Person.bind({ notUsed: true }, 'Charlie');
const p = new Bound(); // new가 우선 적용되어 바인딩된 this는 무시됨
console.log(p.name); // 'Charlie' (생성된 인스턴스가 this)
```


5) 콜백에서의 this 문제와 해결 비교 (setTimeout 등)
```javascript
// JavaScript
const obj = {
  value: 100,
  // 문제: 일반 함수 콜백 -> this는 전역/undefined
  wrong() {
    setTimeout(function() { console.log('wrong:', this && this.value); }, 0);
  },
  // 해결1: arrow -> 렉시컬 this 사용
  usingArrow() {
    setTimeout(() => { console.log('arrow:', this.value); }, 0);
  },
  // 해결2: bind
  usingBind() {
    setTimeout(function() { console.log('bind:', this.value); }.bind(this), 0);
  },
  // 해결3: self 저장
  usingSelf() {
    const self = this;
    setTimeout(function() { console.log('self:', self.value); }, 0);
  }
};
obj.wrong();
obj.usingArrow();
obj.usingBind();
obj.usingSelf();
```


6) 클래스에서의 this와 화살표 함수 필드 (React 스타일)
```javascript
// JavaScript (ES class)
class Counter {
  constructor() {
    this.count = 0;
    // 방법 A: 생성자에서 bind
    this.incBound = this.incBound.bind(this);
  }
  inc() { this.count++; console.log('inc:', this.count); }
  incBound() { this.count++; console.log('incBound:', this.count); }

  // 방법 B: 클래스 필드에 arrow 사용 (프로퍼티 초기화 문법)
  incArrow = () => {
    this.count++;
    console.log('incArrow:', this.count);
  }
}

// 사용 예
const c = new Counter();
const fn = c.inc;       // 메서드를 외부로 빼면 this 상실
fn();                   // undefined 또는 오류
const fnBound = c.incBound;
fnBound();               // 올바르게 동작
const fnArrow = c.incArrow;
fnArrow();               // 올바르게 동작
```


7) 화살표 함수의 잘못된 사용 사례(메서드로 정의하면 안 되는 경우)
```javascript
// JavaScript
const obj = {
  id: 10,
  arrowMethod: () => { console.log(this.id); }, // this는 상위 스코프(글로벌)
  normalMethod() { console.log(this.id); }       // 객체가 this
};

obj.arrowMethod();  // 대부분 undefined (글로벌 this)
obj.normalMethod(); // 10
```


8) 프로토타입/상속 상황에서의 this
```javascript
// JavaScript
function A(x) { this.x = x; }
A.prototype.print = function() { console.log(this.x); };

const a = new A(5);
a.print(); // 5

// 프로토타입 메서드를 다른 객체에 재사용
const bLike = { x: 9 };
A.prototype.print.call(bLike); // 9
```


9) 이벤트 핸들러에서 this (브라우저)
```javascript
// JavaScript (브라우저)
const btn = document.createElement('button');
btn.textContent = 'Click';
document.body.appendChild(btn);

// DOM 이벤트 핸들러의 this는 이벤트를 발생시킨 요소
btn.addEventListener('click', function(event) {
  console.log(this === btn); // true
  console.log('button clicked');
});

// 단, 화살표로 정의하면 this는 상위 스코프(원치 않음)
btn.addEventListener('click', () => {
  // this는 일반적으로 window(또는 상위 렉시컬 스코프)
  console.log('arrow handler this:', this);
});
```


10) 라이브러리 스타일: 콜백에 context 인자로 전달(명시적 설계 패턴)
```javascript
// JavaScript
function runCallback(cb, ctx) {
  // 라이브러리 설계: 콜백을 호출할 때 this에 ctx를 바인딩한다
  return cb.call(ctx);
}

const ctx = { name: 'CTX' };
function show() { console.log(this.name); }
runCallback(show, ctx); // 'CTX'
```


11) 디버깅 예제: this가 왜 바뀌었는지 추적하는 팁
```javascript
// JavaScript
function whoami() {
  console.log('this:', this);
}

// 호출 패턴에 따라 결과가 달라짐
whoami();               // global / undefined (strict)
({method: whoami}).method(); // 호출 객체가 this
whoami.call({foo:1});   // 명시적 바인딩
```


12) 함수 빌드 패턴에서 this를 명확히 하는 법 (의존 주입)
```javascript
// JavaScript
function createService(deps) {
  // this 의존을 줄이고, 명시적 인자로 주입
  return function useService() {
    console.log('using', deps.db);
  };
}
const svc = createService({ db: 'myDB' });
svc(); // 'using myDB'
```


---
