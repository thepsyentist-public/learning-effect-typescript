# 3장 타입스크립트의 타입 시스템

## 아이템22. 타입 좁히기

### 읽은 내용

타입 좁히기의 가장 대표적인 예시는 null 체크
ex)if문으로 null체크하는 방법

타입 좁히기의 방법은
예외를 던지는 방법, instanceof를 사용, 속성 체크, 내장 함수 등으로 좁힐 수 있다.

> 예외를 던지는 방법

```ts
const el = document.getElementById("foo");
if (!el) throw new Error("Error!!!");
el;
el.innerHTML = "hello";
```

> instancof사용

```ts
function contains(text: string, search: string | RegExp) {
  if (search instanceof RegExp) {
    search;
    return !!search.exec(text);
  }
  search;
  return text.includes(search);
}
```

> 속성 체크

```ts
interface A {
  a: number;
}
interface B {
  b: number;
}
function pickAB(ab: A | B) {
  if ("a" in ab) {
    ab; // 타입이 A
  } else {
    ab; // 타입이 B
  }
  ab; // 타입이 A | B
}
```

> 내장 함수

```ts
function contains(text: string, terms: string | string[]) {
  const termList = Array.isArray(terms) ? terms : [terms];
  termList; // string[]
}
```

자바스크립트에서 typeof null은 object이므로

```ts
const el = document.getElementById("foo");
if (typeof el === "object") {
  el;
} // 타입이 따로 좁혀지지 않는다.
```

> 명시적으로 태그를 붙이는 방법

```ts
interface UploadEvent {type: 'upload'; filname:string; contents: string}
interface DownloadEvent {type: 'download'; filname:string; }

type AppEvent = UploadEvent | DownloadEvent

function handleEvent(e: AppEvent) {
  switch(e.type) {
    case 'download':
    e // type이 DownloadEvent
    break;
    case 'upload'
    e // type이 UploadEvent
    break;
  }
}
```

사용자 정의 타입가드

```ts
const group = ["a", "b", "c", "d", "e"];
const members = ["f", "e"].map((who) => group.find((n) => n === who)); // type이 const members: (string | undefined)[]

function isDefined<T>(x: T | undefined): x is T {
  return x !== undefined;
}

const members = ["f", "e"]
  .map((who) => group.find((n) => n === who))
  .filter(isDefined); // type이 const members: string[]
```
