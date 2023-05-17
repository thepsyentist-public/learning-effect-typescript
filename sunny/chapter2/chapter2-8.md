# 2장 타입스크립트의 타입 시스템

## 아이템8. 타입 공간과 값 공간의 심벌 구분하기

### 읽은 내용

타입스크립트 플레이 그라운드를 통해 타입인지 값인지 구분하는 방법이 있다.

```ts
type T1 = "string literal";
type T2 = 123;

const v1 = "string literal";
const v2 = 123;
```

타입에 해당되는부분은 변환된 js에서 보이지 않는다.

```js
"use strict";
const v1 = "string literal";
const v2 = 123;
```

타입 스크립트에서 타입 공간과 값 공간을 혼동하지 않도록하자

```ts
function email({ person: Person, subject: string, body: string }); // 이렇게 쓸게 아니라 아래와 같이

function email({
  person,
  subject,
  body,
}: {
  person: Person;
  subject: string;
  body: string;
}); // 이렇게 써야 잘 동작한다.
```

### 느낀점

- 타입을 선언할때에도 우리가 햇갈릴 수 있는 명명을 하거나, 이미 값으로 정해져있는 명명을 할 경우 혼돈이 올 수 있다. 타입을 선언할때 그런 명명에도 확인을 해야할 필요성이 있고, 잘모르면 타입스크립트 플레이그라운드를 적절히 활용하는것도 좋은 방안이 될 수 있을 것 같다.

### 추가로 공유하고 싶은 내용

[타입스크립트 플레이그라운드](https://www.typescriptlang.org/play?#code/C4TwDgpgBAChBOBnA9gOwKIBsoF5YJVQG0ByAMwEslgSoAfKExCAYzQBMSBdAbgChQkKABUArmEzQ8RRMHgVUAcwA0UVKIC2AIwSqAIgENgEXgPDQxEiFlwjxkouu0JeQA)
