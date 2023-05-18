# 2장 타입스크립트의 타입 시스템

## 아이템15. 동적 데이터에 인덱스 시그니처 사용하기

### 읽은 내용

인덱스 시그니처란

```ts
type Rocket = { [property: string]: string }; // 이부분이 인덱스 시그니처
const rocket: Rocket = {
  name: "a",
  variant: "v2",
  thrust: "333",
};
```

하지만 문제점이 존재

- name대신 Name이어도 유효한 타입이되버린다
- 특정 키가 필요하지 않음 {}도 허용
- 키마다 다른 타입을 가질 수 없다. thrust는 string이 아니라 number여야 할 수도있음
- 타입스크립트 언어서비스가 동작하지 않는다.

인덱스 시그니처로 사용할바엔 interface를 사용한다.

Record란

```ts
//2개가 같은 의미
type Vec3D = Record<"x", "y", "z", number>;

type Vec3D = {
  x: number;
  y: number;
  z: number;
};
```

### 느낀점

- 인덱스 시그니처에 대해 알게되었고, 이것을 사용하기 보다는 인터페이스나 Record 같은 것을 활용하여 정확하게 타입을 사용하는것이 좋은 방법인 것 같다.
