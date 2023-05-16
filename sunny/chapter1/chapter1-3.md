# 1장 타입스크립트 알아보기

## 아이템3. 코드생성과 타입이 관계없음을 이해하기

### 읽은 내용

타입스크립트의 컴파일러는 2가지 역할을 수행

- 최신 Ts/Js를 브라우저에서 동작할 수 있도록 구버전의 자바스크립트로 트랜스파일하기
- 코드의 타입 오류 체크
  위 2가지는 독립적으로 동작

타입 오류가 있는데도 컴파일은 동작한다(but, 설정을 통해서 컴파일 하지 않을 수도 있음)

클래스로 만들면 타입과 값으로 둘다 사용이 가능하다.

```ts
class Square {
  constructor(public width: number) {}
}
class Rectangle extends Square {
  constructor(public width: number, public height: number) {
    super(width);
  }
}
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    // Rectangle이 값으로 사용될 수 있기에 오류가 발생하지 않음
    shpae; // 타입이 Rectangle
    return shape.width * shape.height;
  } else {
    shape; // 타입이 Square
    return shape.width * shape.width;
  }
}
```

타임스크립트에서 함수는 오버로드가 불가능

타입과 타입연산자는 자바스크립트 변환 시점에 제거되기 때문에 런타임 성능에 영향을 주지 않는다.

### 느낀점

- 선언된 타입은 언제든지 달라질 수 있고, 런타임에 타입을 지정하기 위해서는 태그된 유니온이나 속성체크방법, 클래스 등을 사용하기도 한다.
- 아이템 2에서도 느꼈지만, 타입스크립트를 쓴다고해서 무조건 오류를 잡아낼 수 있는것도 아니기에 검증과정은 필수!
