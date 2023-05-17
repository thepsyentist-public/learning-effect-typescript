# 2장 타입스크립트의 타입 시스템

## 아이템9. 타입 단언보다는 타입 선언을 사용하기

### 읽은 내용

```ts
interface Person {
  name: string;
}

const john: Person = { name: "John" }; // 타입 Person
const bob = { name: "Bob" } as Person; // 타입 Person
```

결과는 타입이 Person으로 같아 보이지만 아래코드를 보게되면

```ts
const john: Person = {}; // name 프로퍼티가 없다고 에러, name을 초기화해줘야함.

const bob = {} as Person; // 오류  없음
```

다른 걸 볼 수 있다.

```ts
interface Person {
  name: string;
}

const people = ["a", "b", "c"].map((name) => ({ name }));

/*
결과값이 Person[] 이 아니라 
const people: {
    name: string;
}[] 형태가 된다.
*/

const people: Person[] = ["a", "b", "c"].map((name): Person => ({ name }));
// 명시적으로 Person[]타입임을 표시하게되면, 타입스크립트가 할당문의 유효성을 검사하게된다.
```

타입 단언이 꼭 필요한 경우
타입스크립트는 DOM접근이 불가하여 #myButton이 버튼 엘리먼트인걸 알지 못한다. 명시적으로 타입 단언을해주어야한다.

```ts
document.querySelector("#myButton").addEventListener("click", (e) => {
  e.currentTarget; // 타입은 EventTarget
  const button = e.currentTarget as HTMLButtonElement;
  button; // 타입은 HTMLButtonElement
});
```

! 는 접두사로 쓰이면 boolean의 부정문이지만, 접미사로 쓰이면 타입 단언이 된다

값이 확실히 있다고 하면, !를 통해 타입 단언을 하거나 확신할 수 없을땐 null체크를 해준다

### 느낀점

- 타입스크립트가 올바르게 일을 할 수 있도록 타입을 구체적으로 명시하고, 유효성검사에서 내가 잘못된 부분들을 미리 파악해내는 것 또한 중요한 부분인 것 같다.
- 타입 단언은 지양하면서 null체크를 해줄 수 있는 방식을 사용하는게 좋을 것 같다.
