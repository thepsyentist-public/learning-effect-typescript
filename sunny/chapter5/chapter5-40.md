# 5장 any 다루기

## 아이템40. 함수 안으로 타입 단언문 감추기

### 읽은 내용

```ts
declare function cacheLast<T extends Function>(fn: T): T;

declare function shallowEqual(a: any, b: any): boolean;
function cacheLast<T extends Function>(fn: T): T {
    let lastArgs: any[] | null = null;
    let lastResult: any;

    return function (...args: any[]) {
        //에러 발생 , Type '(...args: any[]) => any' is not assignable to type 'T'.
        if (!lastArgs || !shallowEqual(lastArgs, args)) {
            lastResult = fn(...args);
            lastArgs = args;
        }
        return lastResult;
    };
}
```

타입스크립트는 반환문에 있는 함수와 원본 함수 T타입이 어떤 관련있는지 알지 못하기 때문에 오류가 발생
원본 함수 T타입과 동일한 매개변수로 호출되고 반환 값 또한 예상한 결과가 되기 때문에
return에 as unknown as T 만 추가해주면 에러 해결

타입 선언문이 상황에 따라 필요하기도, 해결책이 되기도 한다.
만약 사용해야한다면, 정확한 정의를 가지는 함수 안으로 숨기도록한다.
