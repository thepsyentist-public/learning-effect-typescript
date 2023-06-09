# 4장 타입스크립트의 타입 시스템

## 아이템30. 문서에 타입 정보를 쓰지 않기

### 읽은 내용

주석과 변수명에 타입 정보를 적는 것을 피하자.

```ts
// 전경색 문자열을 반환한다.
// 0개 또는 1개의 매개변수를 받는다.
// 매개변수가 없을 때느 표준 전경색을 반환한다.
// 매개변수가 있을 때는 특정 페이지의 전경색을 반환한다.

function getForegroundColor(page?: string) {
    return page === "login" ? { r: 127, g: 127, b: 127 } : { r: 0, g: 0, b: 0 };
}
```

위 주석의 내용의 문제점

-   함수가 string을 반환한다고 하지만, 실제론 객체를 반환한다.
-   주석에 0개 또는 1개의 매개변수를 받는다고 하는데 타입 시그니처만 봐도 알 수 있는 정보이다.
-   불필요하게 장황하다.

다음과 같이 개선해볼 수 있다.

```ts
// 어플리케이션 또는 특정 페이지의 전경색을 가져옵니다.
function getForegroundColor(page?: string): Color {
    return page === "login" ? { r: 127, g: 127, b: 127 } : { r: 0, g: 0, b: 0 };
}
```

타입이 명확하지 않은 경우는 변수명에 단위정보를 포함하는 것도 고려하자.
ex) timeMs 또는 temperatureC