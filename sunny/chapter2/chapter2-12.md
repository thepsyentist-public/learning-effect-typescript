# 2장 타입스크립트의 타입 시스템

## 아이템12. 함수 표현식에 타입 적용하기

### 읽은 내용

매개변수나 반환값에 타입을 명시하기보다는 함수 표현식 전체에 타입 구문을 적용 하는 것이 좋다.

```ts
declare function fetch(
  input: RequestInfo,
  init?: RequestInit
): Promise<Response>;

async function checkdFetch(input: RequestInfo, init?: RequestInit) {
  const response = await fetch(input, init);
  if (!reponse.ok) {
    // 비동기 함수 내에서 거절된 프로미스로 변환함
    throw new Error("Request failed: " + response.status);
  }
  return response;
}

// 위코드도 잘 동작하겠지만, 아래 코드가 좀 더 좋다

const checkdFetch: typeof fetch = async (input, init) => {
  const response = await fetch(input, init);
  if (!response.ok) {
    // 비동기 함수 내에서 거절된 프로미스로 변환함
    throw new Error("Request failed: " + response.status);
  }
  return response;
};

// 함수표현식으로 바꿨고, 함수 전체에 타입적용으로 타입스크립트가 input과 init의 타입을 추론할수있게함
```

### 느낀점

- 함수 표현식에서 type을 사용할때 타입 지정을 잘 사용하지 않았던 것 같은데, 위와같은 식으로 typeof 를 활용하여 함수표현식전체에 타입을 지정해줄 수 있는 부분을 알았고, 해당 부분을 앞으로 활용하면서 코딩해야겠다라고 생각했다.
