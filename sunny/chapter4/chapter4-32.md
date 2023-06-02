# 4장 타입스크립트의 타입 시스템

## 아이템32. 유니온의 인터페이스보다는 인터페이스의 유니온을 사용하기

### 읽은 내용

```ts
interface FillLayer {
    type: "fill";
    layout: FillLayout;
}

interface LineLayer {
    type: "line";
    layout: LineLayout;
}

interface PointLayer {
    type: "point";
    layout: PointLayout;
}
type Layer = FillLayer | LineLayer | PointLayer;
```

type속성은 태그 이며 런타임 때 어떤 타입의 Layer가 사용되는지 판단하는데 쓰이며, 타입스크립트는 이 태그를 참고하여 Layer의 타입의 범위를 좁힐수도있습니다.

> 두객체의 속성을 하나의 객체로 모으는 것이 더 낫다.

```ts
interface Person {
    name: string;
    placeOfBirth?: string;
    dateOfBirth?: Date;
}

// 위 코드 보단 아래코드로
interface Person {
    name: string;
    birth?: {
        place: string;
        date: Date;
    };
}
```

인터페이스의 유니온을 사용하여 속성 사이의 관계를 모델링하기.
아래와같이 PersonWithBirth는 Name을 확장하여 사용할수도 있다.

```ts
interface Name {
    name: string;
}

interface PersonWithBirth extends Name {
    placeOfBirth: string;
    dateOfBirth: Date;
}

type Person = Name | PersonWithBirth;
```

### 느낀점

-
