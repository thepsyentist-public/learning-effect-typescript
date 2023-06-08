# 5장 any 다루기

## 아이템42. 모르는 타입의 값에는 any대신 unknown을 사용하기

### 읽은 내용

함수의 반환타입은 any를 사용하는 것 보다 unknown으로 사용하는 것이 낫다.

any가 강력하면서도 위험한 이유

1. 어떠한 타입이든 any타입에 할당 가능하다
2. any타입은 어떠한 타입으로도 할당 가능하다

unknown은 1번은 만족 / 2번은 x
never는 1번 x / 2번은 만족

unknown 타입을 원하는 타입으로 변환하는 방법

```ts
function processValue(val: unknown) {
    if (val instanceof Date) {
        val; // 타입이 Date
    }
}
```

```ts
declare const foo: Foo;
let barAny = foo as any as Bar;
let barUnk = foo as unknown as Bar;
```

기능적으로 동일하나, unknown형태가 더 안전하다.
분리되는 즉시, 오류를 발생시켜서 안전하다

{}타입은 null과 undefined를 제외한 모든 값을 포함한다.
object타입은 모든 비기본형 타입으로 이루어진다. true 또는 12 또는 "foo" 가 포함되지 않지만 객체와 배열은 포함된다.

어떠한 값이 있지만, 타입을 모를때 unknown을 사용하자
사용자가 타입 단언문이나 타입 체크를 사용하도록 강제하려면 unknown을 사용하자.
