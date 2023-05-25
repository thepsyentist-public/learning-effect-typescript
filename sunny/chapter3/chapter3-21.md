# 3장 타입스크립트의 타입 시스템

## 아이템21. 타입 넓히기

### 읽은 내용

타입 넓히기 : 타입체커가 타입을 결정해야하는데, 지정된 단일 값을 가지고 할당 가능한 값들의 집합을 유추해내는 과정

```ts
interface Vector3 {
  x: number;
  y: number;
  z: number;
}
function getComponent(vector: Vector3, axis: "x" | "y" | "z") {
  return vector[axis];
}

let x = "x";
let vec = { x: 10, y: 20, z: 30 };

getComponent(vec, x); //에러발생,  Argument of type 'string' is not assignable to parameter of type '"x" | "y" | "z"'
```

위에서 선언한 let x 의 타입이 string 으로 추론되어서 에러발생

```ts
const mixed = ["x", 1]; // 이 코드 하나도 타입스크립트는 다양한 타입으로 추론이 된다
```

타입스크립트 넓히기 과정을 제어할 수 있는 방법

- const

```ts
const x = "x"; // 타입이 'x'이다. let과 다르게 const는 타입 체커가 통과됨
let vec = { x: 10, y: 20, z: 30 };
getComponent(vec, x);
```

하지만, const는 만능이 아니다. 앞의 const mixed 예제에서만 봐도 배열에 대한 문제가있음.

객체에서의 타입스크립트 넓히기 알고리즘은 각 요소를 let으로 할당한 것과 같다.

```ts
const v = {
  x: 1,
}; // x:number 타입으로 추론
v.x = 3; // 성공
v.x = "3"; // 에러
v.y = 4; // 에러
v.name = "name"; // 에러
```

타입 추론의 강도를 직접 제어하기 위해서는 기본 동작을 재정의 해야함!

> 기본동작 재정의 하는 3가지 방법

1. 명시적 타입 구문 제공

```ts
const v: { x: 1 | 3 | 5 } = {
  x: 1,
};
```

2. 타입 체커에 추가적인 문맥 제공 (함수 매개변수로 값을 전달)
3. const 단언문 사용

```ts
const v1 = {
  x: 1,
  y: 2,
}; // x: number , y:number

const v2 = {
  x: 1 as const,
  y: 2,
}; // x:1; y:number

const v3 = {
  x: 1,
  y: 2,
} as const; // readonly x: 1, readonly y:2

const a1 = [1, 2, 3]; // number[]
const a2 = [1, 2, 3] as const; // readonly [1,2,3]
```

### 느낀점

- 타입스크립를 사용하면서 as const 를 보기만했었지 정확한 의미를 잘 몰랐는데 마지막 예제를 통해서 알 수 잇었던 것 같다. 또한, 기본동작을 잘 제어해서 오류가 생기는 일을 방지하는 것 또한 중요한 부분 일 것 같다.
