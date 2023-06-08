# 5장 any 다루기

## 아이템39. any를 구체적으로 변형해서 사용하기

### 읽은 내용

any타입을 쓸때 쓰더라도 이런식으로

```ts
function getA(array: any) {
    return array.length;
}
function getB(array: any[]) {
    return array.length;
}
```

위와 같이 any[]로 썼을 경우에

1. 함수 내의 array.length타입이 체크됨
2. 함수의 반환타입이 any대신 number로 추론됨
3. 함수 호출될 때 매개변수가 배열인지 체크됨

매개변수가 객체이긴 하지만 값을 알 수 없다면 {[key: string]: any} 형태로

```ts
function hasA(o: {[key: string]: any}) {...}
```

object라고 사용할 수도 있지만, 속성에 접근 할 수 없다는 점에서 다르다.

```ts
type Fn0 = () => any; // 매개변수 없이 호출 가능한 모든 함수
type Fn1 = (ar: any) => any; // 매개변수 1개
type FnN = (...args: any[]) => any; // 모든 개수의 매개변수 , Fuction타입과 동일
```

any를 사용할때는 구체적인 any형태를 사용하는 것이 좋다.
