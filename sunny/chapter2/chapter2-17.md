# 2장 타입스크립트의 타입 시스템

## 아이템17. 변경 관련된 오류 방지를 위해 readonly 사용하기

### 읽은 내용

readonly

readonly number[]는 number[] 서브타입이 된다.

매개변수를 readonly로 할경우

- 타입스크립트는 매개변수가 함수 내에서 변경이 일어나는지 체크
- 호출하는 쪽에서는 함수가 배개변수를 변경하지 않는다는 보장을 받게됨
- 호출하는 쪽에서 함수에 readonly배열을 매개변수로 넣을 수도있다.

매개변수를 수정하지 않을거라면 readonly를 붙이면 인터페이스를 명확하게 할 수 있고, 변경할떄 에러를 발생시키기에 오류확인할때 좋다!

const 와 readonly의 차이

const는 변수 참조를 위한 것, 변수에 다른 값을 할당할 수 없다.

readonly는 속성을 위한 것

```ts
type readonlyA = {
  readonly barA: string;
};

const x: readonlyA = { barA: "baz" };
x.barA = "quux"; // ⛔️ 변경 불가
```

```ts
type readonlyB = {
  readonly barB: { baz: string };
};

const y: readonlyB = { barB: { baz: "quux" } };
y.barB.baz = "zebranky"; // 👌 변경 가능
```

이예제가 가능한 이유는 readonly가 얕게 동작하기 때문
->barB가 참조하고 있는 값 자체는 변경될 수 없지만
얘가 readonly라고 그 안에 있는 속성들이 모두 동일한 접근 제어자를 가지고 있는 것이 아니다.

### 느낀점

- readonly를 사용할떄는 클래스형에서 변수선언시에 주로 사용했었는데, 타입에서도 가능한지는 잘 몰랐었다. 타입에서도 이런식으로 사용하게되면, 실수나 오류방지에 좋을 것 같다.
