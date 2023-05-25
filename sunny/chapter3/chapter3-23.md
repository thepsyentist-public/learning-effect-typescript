# 3장 타입스크립트의 타입 시스템

## 아이템23. 한꺼번에 객체 생성하기

### 읽은 내용

> 타입추론에 유리하기 위해서는 객체를 생성시에 한꺼번에 생성해야 유리하다.

```ts
const pt = {};
pt.x = 3; // 오류가 발생

// 아래와같이 한꺼번에 만들기!
interface Point {
  x: number;
  y: number;
}
const pt: Point = {
  x: 3,
  y: 4,
};
```

객체 전개 연산자 활용하면 타입 걱정 없이 필드 단위로 객체를 생성 할 수도 있다.

```ts
const pt = { x: 3, y: 4 };
const id = { name: "abc" };

const namedPoint = {};
Object.assign(namedPoint, pt, id); // 이렇게 쓰는 것 보다

namedPoint.name; // 에러

const namedPoint = { ...pt, ...id }; // 전개연산자를 활용해야한다.
namedPoint.name; // 정상
```

> 객체나 배열을 변환해서 새로운 객체나 배열 생성시엔 루프 대신에 내장함수나 Lodash 활용하기!
