# 2장 타입스크립트의 타입 시스템

## 아이템10. 객체 래퍼 타입 피하기

### 읽은 내용

타입스크립트는 기본형과 객체 래퍼 타입을 별도로 모델링한다.

```ts

// 기본형 , 객체래퍼
// 타입에서는  기본형을 써야한다.
string String
number Number
boolean Boolean
symbol Symbol
bigint BigInt

// 특히 조심해서 사용해야한다.

function getStringLent(foo:String) {
    return foo.length;
}

getStringLen("hello");
getStringLen(new String("hello"));

// 이 경우는 크게 문제 될 건 없다.
// but,

function getStringLent(foo:String) {
    return ['hello','good'].includes(foo);
    // Argument of type 'String' is not assignable to parameter of type 'string'.
    // 'string' is a primitive, but 'String' is a wrapper object. Prefer using 'string' when possible.(2345)
    // 에러가 발생
}

```

### 느낀점

- 가끔 객체래퍼와 타입에 대해서 타이핑을 하던 도중 실수 하는 경우가 있곤 했는데, 이 챕터에서 잘 캐치해준 것 같다. 두개를 혼동해서 사용하였을때
  문제가 없는 것처럼 보일 수 있기때문에, 코드리뷰 등을 통하여 검증해보는 과정 또한 필요할 것 같다.

### 추가로 공유하고 싶은 내용

잘 사용하지는 않지만, BigInt 타입과 Symbol 타입에 대해서 살펴보면,

BigInt타입은 number로 표현하기엔 너무 큰 숫자 값을 나타낸다.

```ts
const previouslyMaxSafeInteger = 9007199254740991n;

const alsoHuge = BigInt(9007199254740991);
// 9007199254740991n

const hugeString = BigInt("9007199254740991");
// 9007199254740991n

const hugeHex = BigInt("0x1fffffffffffff");
// 9007199254740991n

const hugeOctal = BigInt("0o377777777777777777");
// 9007199254740991n

const hugeBin = BigInt(
  "0b11111111111111111111111111111111111111111111111111111"
);
// 9007199254740991n
```

심볼(symbol)은 ES6에서 새롭게 추가된 7번째 타입으로 변경 불가능한 원시 타입의 값이다.
심볼은 주로 이름의 충돌 위험이 없는 유일한 객체의 프로퍼티 키(property key)를 만들기 위해 사용한다.
[symbol타입](https://poiemaweb.com/es6-symbol)
