# ECMA Script

---

## 배경 
- 크로스 브라우저 지원 위해서 나옴 


---

## Default Function Parameters


---


## Rest Parameters (JAVA 에선 가변 인자)
 
- ...args : 맨 마지막에만 올 수 있다.
- function 함수이름 (...args) {}
- arrow function 에서는 사용 x
- 배열로 변환 가능 
  - var arr = Array.prototype.slice.call(arguments);
  - var arr = Array.from(arguments); // ES6
  - var arr = [...arguments]; // spread operator



---

## Arrow Function

- 항상 this 가리키는는거 유의 하면서 코딩하기

---

## 계산된 프로퍼티 이름 
- 동일 이름일땐 재대입,
---

## 스프레드 연산자 
- 객체 복사(복제) DeepCopy
- myFuntion(...literable)  // 호출시에 ... 으로 호출
  - vs Rest Parameters : 스프레드 : 호출시, REST : 선언시 
- 
