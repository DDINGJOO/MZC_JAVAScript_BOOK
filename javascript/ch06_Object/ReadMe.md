# 객체와 배열

---

- 개요
  - 자바스크립트의 핵심은 객체(Object)입니다. 대부분의 값이 객체이거나 객체처럼 동작합니다.
  - 배열(Array)은 순서가 있는 컬렉션을 표현하는 특수한 객체입니다.

- 관련 실습 파일
  - Object01.html: 객체 생성과 프로퍼티 접근의 기본
  - Array_01.html ~ Array_09.html: 배열 생성, 탐색, 수정 메서드 실습 전반

- 객체(Object)
  - 리터럴로 생성: const user = { name: 'Kim', age: 20 };
  - 프로퍼티 접근: user.name, user['age']
  - 동적 추가/삭제: user.job = 'dev'; delete user.age;
  - in 연산자 / hasOwnProperty로 존재 여부 확인
  - 얕은/깊은 복사 주의: Object.assign, 구조분해, JSON 기반 복사 등

- 배열(Array)
  - 생성: const arr = [1,2,3]; new Array(3) 등
  - 길이: arr.length (가변)
  - 주요 반복: for, for...of, forEach, map, filter, reduce
  - 추가/삭제: push, pop, unshift, shift, splice
  - 정렬/검색: sort, reverse, indexOf, includes, find, findIndex
  - 변환: join, slice(얕은복사), concat, flat

- 불변 스타일 팁
  - 기존 배열을 변경하는(mutable) 메서드(splice, sort 등) 대신, 사본을 만들어 처리하는 방식을 습관화하면 버그를 줄일 수 있습니다.

- 예시
```javascript
// 객체 조작
const user = { name: 'Kim' };
user.age = 20;
console.log('age' in user); // true

// 배열 변환
const nums = [1,2,3,4];
const evens = nums.filter(n => n % 2 === 0); // [2,4]
const doubled = nums.map(n => n * 2);        // [2,4,6,8]
```

---

- 보완 필요 항목 목록(_01.md 등으로 채워나가기)
  - _01.md: 배열 고급 메서드 활용 패턴(immutable vs mutable) 정리
  - _02.md: 얕은/깊은 복사 사례와 구조 분해 패턴 모음
