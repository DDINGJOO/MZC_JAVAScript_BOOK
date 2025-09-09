# JSON

---

- 개요
  - JavaScript Object Notation: 경량 데이터 교환 포맷
  - 텍스트 기반, 언어 독립적, 대부분의 언어에서 파서 제공

- 문법 요소
  - 객체: { "key": value }
  - 배열: [ 1, 2, 3 ]
  - 값 타입: number, string, boolean, null, object, array (함수/undefined는 불가)
  - 문자열은 반드시 쌍따옴표

- 직렬화/역직렬화
  - JSON.stringify(obj[, replacer[, space]])
  - JSON.parse(text[, reviver])

- 실습 파일
  - JSON_01.html: stringify/parse 기본 동작 확인

- 예시
```javascript
const obj = { name: 'Kim', age: 20, tags: ['js','web'] };
const text = JSON.stringify(obj, null, 2);
console.log(text);

const revived = JSON.parse(text);
console.log(revived.name);
```

- 주의
  - Date, Map, Set, 함수 등은 JSON으로 직접 표현되지 않습니다. 필요 시 커스텀 직렬화(replacer) 또는 별도 포맷 사용.
