# 2장 타입스크립트의 타입 시스템

## 아이템13. 타입과 인터페이스의 차이점 알기

### 읽은 내용

타입과 인터페이스의 차이

```ts
type TState = {
  name: string;
  capital: string;
};

interface IState {
  name: string;
  capital: string;
}
```

인덱스 시그니처, 함수 타입, 제네릭 모두 사용하는데 크게 차이가 없음

interface는 타입을 확장할 수 있고, 타입은 인터페이스를 확장 가능 하다.

```ts
interface IstateWithPop extends Tstate {
  population: number;
}
type TstateWithPop = IState & { population: number };
```

인터페이스는 유니온 타입 같은 복잡한 타입을 확장하지는 못한다. -> 타입을 사용하여야한다.

타입과 인터페이스의 차이점

- 유니온 타입은 있지만 유니온 인터페이스는 없다.
- 인터페이스는 보강이 가능하다-선언병합이 가능(아래예제)

```ts
interface IState {
  name: string;
  capital: string;
}
interface IState {
  poplulation: number;
}
const wyoming: IState = {
  name: "ming",
  capital: "seoul",
  population: 500,
}; // 에러가 발생하지 않고 잘 동작한다.
```

복잡한 타입의 경우 타입으로 표현하는게 맞지만, 그게 아닐 경우 interface, type 둘 중에 고민해본다.

### 느낀점

- 사실 타입을 사용하기보다는 그렇게 복잡한 타입을 사용할일이 없어서 인터페이스를 많이 사용했었는데, 조금 찾아봤을때 객체에서만 쓰는 용도가 많기 때문에 인터페이스를 쓰는게 적합하다고 한다. 앞으로도 사실 복잡한 타입을 가지는 경우가 아니면 왠만하면 인터페이스로 맞춰서 쓰는게 좋을 것 같긴하다.

### 추가로 공유하고 싶은 내용

interface 들을 합성할 경우 이는 캐시가 되지만, 타입의 경우에는 그렇지 못하다.

타입 합성의 경우, 합성에 자체에 대한 유효성을 판단하기 전에, 모든 구성요소에 대한 타입을 체크하므로 컴파일 시에 상대적으로 성능이 좋지 않다.
