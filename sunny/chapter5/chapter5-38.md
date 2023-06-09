# 5장 any 다루기

## 아이템38. any타입은 가능한 한 좁은 범위에서만 사용하기

### 읽은 내용

```ts
function processBar(b: Bar) {}

function f() {
    const x: any = expressionReturningFoo(); // any를 이렇게 쓰기보다는
    processBar(x);
}

function f() {
    const x = expressionReturningFoo();
    processBar(x as any); // any를 이렇게 쓰는게 낫다 => 다른 코드들에 영향을 끼치지 않기 때문에
}
```

타입스크립트가 함수의 반환 타입을 추론할 수 있는 경우에도 함수 반환 타입을 명시하는 것이 좋다.
any타입이 함수 바깥으로 영향을 미치는 것을 방지할 수 있다.

```ts
function f() {
    // @ts-ignore   // 이런식으로 오류를 해결 할 수 있다.
    const x = expressionReturningFoo();
    processBar(x as any);
}
```

최소한의 범위에만 any를 사용하는 것이 좋다.

```ts
const config: Config = {
  a: 1,
  b: 2,
  c: {
    key:value
  }
} as any;  // 이것보다는

const config: Config = {
  a: 1,
  b: 2,
  c: {
    key:value as any;  // 이게 나음
  }
}
```
