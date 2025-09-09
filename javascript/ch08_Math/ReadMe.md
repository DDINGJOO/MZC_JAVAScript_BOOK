# Math 객체

---

- 개요
  - 수학 연산을 위한 정적 메서드와 상수를 제공합니다.
  - Math는 생성자가 없고 모두 정적(static)입니다.

- 상수
  - Math.PI, Math.E, Math.SQRT2, Math.LN2 등

- 주요 메서드
  - 올림/내림/반올림: ceil, floor, round, trunc
  - 절대값/부호: abs, sign
  - 제곱/제곱근: pow, sqrt
  - 난수: random() // 0이상 1미만
  - 최솟값/최댓값: min, max

- 실습 파일
  - Math_01.html: 주요 메서드 동작 확인

- 예시
```javascript
// 1~6 주사위
const dice = Math.floor(Math.random() * 6) + 1;

// 반올림 자리 지정
function roundTo(n, digits = 0) {
  const p = 10 ** digits;
  return Math.round(n * p) / p;
}
console.log(roundTo(3.14159, 2)); // 3.14
```
