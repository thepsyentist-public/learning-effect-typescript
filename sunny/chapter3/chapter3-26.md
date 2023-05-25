# 3장 타입스크립트의 타입 시스템

## 아이템26. 타입 추론에 문맥이 어떻게 사용되는지 이해하기

### 읽은 내용

```ts
type Language = "Javascript" | "TypeScript" | "Python";
function setLanguage(language: Language) {}

setLanguage("Javascript"); // 정상

let language = "Javascript";
setLanguage(language); // 에러 , Argument of type 'string' is not assignable to parameter of type 'Language'
// 변수로 분리해내면 , 타입스크립트는 할당 시점에 타입을 추론한다.

let language: Language = "Javascript"; // 이렇게 타입 선언에서 가능한 값을 제한하거나
setLanguage(language);

const language = "Javascript"; // let 대신에 const 를 사용하여 타입 체커에게 language 는 변경불가라고 알려줌
setLanguage(language);
```

> 튜플 사용 시 주의점

```ts
function panTo(where: [number, number]) {}

panTo([10, 20]);

const loc = [10, 20];
panTo(loc); // 에러 발생 , Argument of type 'number[]' is not assignable to parameter of type '[number, number]'.

//아래와 같이 수정 가능

const loc: [number, number] = [10, 20]; // 타입 선언
panTo(loc); // 정상

// 타입 단언을 사용할 수 있긴하지만,
const loc = [10, 20] as const;
panTo(loc); // 에러 발생,  Argument of type 'readonly [10, 20]' is not assignable to parameter of type '[number, number]'.

// 함수를 수정하는게 낫다.
function panTo(where: readonly [number, number]) {}

// 하지만, 타입 정의에 실수가 있었다고 한다면,  오류가 발생할 수 있다.
```

> 객체 사용시 주의점

```ts
type Language = "Javascript" | "TypeScript" | "Python";

interface GovernedLanguage {
  lanuage: Language;
  organize: string;
}

function complain(language: GovernedLanguage) {}

complain({ lanuage: "TypeScript", organize: "microsoft" });

const ts = {
  language: "Typescript",
  organize: "microsoft",
};
// 타입 추론
// const ts: {
//     language: string;
//     organize: string;
// }

complain(ts); // 에러 발생, Argument of type '{ language: string; organize: string; }' is not assignable to parameter of type 'GovernedLanguage'.

const ts: GovernedLanguage = {
  // 타입선언 추가해줘야함
  language: "Typescript",
  organize: "microsoft",
};
```

> 콜백 사용시 주의점

```ts
function callWithRandomNunbers(fn: (n1: number, n2: number) => void) {
  fn(Math.random(), Math.random());
}

callWithRandomNunbers((a, b) => {
  a; // 타입 number
  b; // 타입 number
  console.log(a + b);
});

const fn = (a, b) => {
  // 에러 발생 Parameter 'a' implicitly has an 'any' type., Parameter 'b' implicitly has an 'any' type.
  console.log(a + b);
};
callWithRandomNunbers(fn);

const fn = (a: number, b: number) => {
  // 매개변수에 타입 구문을 추가해 해결 가능
  console.log(a + b);
};
callWithRandomNunbers(fn);
```
