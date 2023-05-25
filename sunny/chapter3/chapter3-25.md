# 3장 타입스크립트의 타입 시스템

## 아이템25. 비동기 코드에는 콜백 대신 async함수 사용하기

### 읽은 내용

```ts
function timeout(millis: number): Promise<never> {
  return new Promise((resolve, reject) => {
    setTimeout(() => reject("timeout"), millis);
  });
}

async function fetchWithTimeout(url: string, ms: number) {
  return Promise.race([fetch(url), timeout(ms)]);
}

// 타입 구문이 없어도 반환 타입은  Promise<Response>이다.
```

Promise도 좋지만, async await를 사용하기

- 코드가 간결하고 직관적
- async함수는 항상 프로미스를 반환하도록 강제함

```ts
function getNumber(): Promise<number>;
async function getNumber() {
  return 42;
}
const getNumber = async () => 42; // 간단한 형태로 만들수있음
```

### 추가

Promise.race() 메소드는 Promise 객체를 반환합니다.
이 프로미스 객체는 iterable 안에 있는 프로미스 중에 가장 먼저 완료된 것의 결과값으로 그대로 이행하거나 거부합니다.
