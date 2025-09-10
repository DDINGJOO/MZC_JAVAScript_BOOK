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
