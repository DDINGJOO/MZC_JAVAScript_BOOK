# Date 객체

---

- 개요
  - 날짜와 시간을 다루는 내장 객체입니다.
  - 타임존, 로케일, 서머타임 등 이슈가 있으므로 포맷팅과 계산 시 주의합니다.

- 생성
  - new Date() // 현재
  - new Date(ms) // Epoch(ms)
  - new Date('2025-09-09T09:00:00Z') // ISO 문자열

- 주요 메서드
  - getFullYear(), getMonth(0~11), getDate(), getDay(0=일)
  - getHours(), getMinutes(), getSeconds(), getMilliseconds()
  - setFullYear(...), setMonth(...), setDate(...)
  - toISOString(), toLocaleString(), toDateString()

- 실습 파일
  - Date_01.html: Date 기본 생성과 출력 실습

- 예시
```javascript
const now = new Date();
console.log(now.toISOString());

const d = new Date('2025-09-09T00:00:00');
d.setDate(d.getDate() + 7); // +7일
console.log(d.toLocaleString());
```

- 팁
  - 월은 0부터 시작합니다(0=1월). 오프바이원 주의.
  - 날짜 연산은 setDate/getDate 조합으로 수행하거나 라이브러리를 고려(moment/dayjs 등).
