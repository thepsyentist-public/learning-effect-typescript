# 2장 타입스크립트의 타입 시스템

## 아이템7. 타입이 값들의 집합이라고 생각하기

### 읽은 내용

타입은 엄격한 상속관계가 아니라 겹쳐지는 집합으로 표현된다.

extends

```ts
interface Person {
  name: string;
}

interface Personspan extends Person {
  //  Personspan은  Person의 부분집합, 서브집합
  birth: Date;
  death?: Date;
}

function getKey<k extends string>(val: any, key: K) {
  //extends 제네릭타입에서 한정자로 쓰임
  //...
}

getKey({}, "x"); // 정상
getKey({}, Math.random() < 0.5 ? "a" : "b"); // 결과값이 string이므로 정상
getKey({}, document.title); // string값이므로 정상
getKey({}, 12); // 12는 불가
```

| 타입스크립트 용어         | 집합 용어                   |
| ------------------------- | --------------------------- |
| never                     | 공집합                      |
| 리터럴 타입               | 원소가 1개인 집합           |
| 값이 T에 할당 가능        | 값 ∈ T(값이 T의 원소이다)   |
| T1이 T2에 할당 가능       | T1 ⊆ T2(T1이 T2의 부분집합) |
| T1이 T2를 상속            | T1 ⊆ T2(T1이 T2의 부분집합) |
| T1ㅣT2(T1과 T2의 유니온)  | T1 ∪ T2 합집합              |
| T1&T2(T1과 T2의 인터섹션) | T1 ∩ T2 교집합              |
| unknown                   | 전체 집합                   |

### 느낀점

- 타입스크립트를 사용하면서 수학의 집합과 닮은 것들이 많다고 느끼고 있었는데, 역시나 비슷한 점이 많은것 같다.
  extends를 잘 활용하여 상속 타입들을 사용할 수 있는 점이 큰 메리트 인 것 같다.
