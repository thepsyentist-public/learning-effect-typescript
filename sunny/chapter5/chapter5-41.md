# 5장 any 다루기

## 아이템41. any의 진화를 이해하기

### 읽은 내용

```ts
futcion range(start:number, limit:number) {
  const out = [];  // any[] 로 선언되었지만, number타입을 넣는 순간 타입이 진화함
  for(let i=start;i<limit ;i++) {
    out.push(i)
  }
  return out;  // 반환타입이 numer[]로 추론
}
```

타입의 진화 예제

```ts
const result = []; // 타입 any[]
result.push("a");
result; // 타입 string[]
result.push(1);
result; // 타입 (string | number)[]
```

any타입의 진화는 noImlicitAny가 설정된 상태에서 변수의 타입이 암시적 any인 경우에만 일어남

하지만, 명시적으로 any 선언하면 any로 유지된다.

any를 진화시키는 방식보다는 명시적 타입 구문을 사용하는 것이 좋은 설계이다.
