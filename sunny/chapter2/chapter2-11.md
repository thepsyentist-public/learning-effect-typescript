# 2장 타입스크립트의 타입 시스템

## 아이템11. 잉여 속성 체크의 한계 인지하기

### 읽은 내용

```ts
interface Room {
  numDoors: number;
  ceilingHeightFt: number;
}

const r: Room = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: "present", // 에러발생
  // Type '{ numDoors: number; ceilingHeightFt: number; elephant: string; }' is not assignable to type 'Room'.
};
```

위 예제에서 구조적타이핑 관점에서만 보면 에러가 발생하지 않아야 하지만, 에러가 발생함 -> 잉여속성체크

```ts
const obj = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: "present", // 에러발생
};

const r: Room = obj;
```

이것은 정상적으로 동작하는 이유는 obj 객체의 타입추론이

```ts
const obj = {
  numDoors: number,
  ceilingHeightFt: number,
  elephant: string,
};
```

로 추론되기 때문에(obj타입은 Room타입의 부분집합을 포함하므로) 할당이 가능하고, 타입체커도 통과하게 된다.

```ts
interface Options {
  title: string;
  darkMode?: boolean;
}

const o: Options = { darkmode: true, title: "ski" }; // 에러발생 , 객체는 잉여속성체크가 된다

//Type '{ darkmode: boolean; title: string; }' is not assignable to type 'Options'.
//Object literal may only specify known properties, but 'darkmode' does not exist in type 'Options'.
// Did you mean to write 'darkMode' ?
```

```ts
const intermediate = { darkmode: true, title: "ski" }; // 객체 리터럴
const o: Options = intermediate; // 에러발생하지않음, 객체 리터럴이 아님, 잉여속성체크가 되지 않고 오류가 사라짐
```

```ts
const o = { darkmode: true, title: "ski" } as Options; // 타입 단언을 사용해도 오류가 사라짐
```

잉여속성체크를 해제하는 방법

```ts
interface Options {
  darkMode?: boolean;
  [otherOptions: string]: unknown;
}

const o: OPtions = { darkmode: true }; // 정상적으로 동작함
```

### 느낀점

- 잉여속성체크가 오류를 찾아내주는 좋은 방법이고, 내가 얘기치못하게 잘못코딩하는 부분에 있어서도 잡아줄 수 있는 점이 좋은 것 같다.
