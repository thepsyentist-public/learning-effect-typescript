# 2장 타입스크립트의 타입 시스템

## 아이템14. 타입 연산과 제너릭 사용으로 반복 줄이기

### 읽은 내용

타입의 중복 줄이기

```ts
function distance(a: {x:number, y:number}, b: {x:number, y:number}) {
    ...
}

interface Point2D{
    x:number
    y:number
}
function distance(a:Point2D, b:Point2D) {...}
```

```ts
function get(url:string, opts:Options) : Promise<Reseponse> {...}
function post(url:string, opts:Options) : Promise<Reseponse> {...}

type HTTPFunction = (url:string, opts:Options) => Promise<Response>;
const get: HTTPFuction = (url, opts) => {...}
const post: HTTPFuction = (url, opts) => {...}
```

중복을 제거하는 과정 및 Pick

```ts
interface State {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
  pageContents: string;
}

interface TopNavState {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
}

// 위 코드를 아래 형태로

type TopNavState = {
  userId: State["userId"];
  pageTitle: State["pageTitle"];
  recentFiles: State["recentFiles"];
};

// 아래와 같은 형식으로 중복 제거

type TopNaveState = {
  [k in "userId" | "pageTitle" | "recentFiles"]: State[k];
};

// Pick이라는 제너릭타입을 활용 하여 중복 없애기

type TopNavState = Pick<State, "userId" | "pageTitle" | "recentFiles">;
```

```ts
interface Options {
  width: number;
  height: number;
  color: string;
  label: string;
}

interface OPtionsUpdate {
  width?: number;
  height?: number;
  color?: string;
  label?: string;
}

class UIWidget {
  constructor(init: Options) {}
  update(options: OPtionsupdate) {}
}
```

위코드에서 keyof를 사용하면 OptionsUpdate를 만들 수 있다.

```ts
type OptionsUpdate = { [k in keyof Options]?: Options[k] };
```

Partial의 활용

```ts
class UIWidget {
  constructor(init: Options) {}
  update(options: Partial<Options>) {}
}
```

제너릭 타입의 매개변수 제한할 수 있는 방법은 extends를 사용하는것
extends를 이요함녀 제너릭 매개변수가 특정 타입을 확장한다고 선언할 수 있다.

```ts
interface Name {
  first: string;
  last: string;
}

type DancingDuo<T extends Name> = [T, T];

const couple1: DancingDuo<Name> = [
  { first: "Fred", last: "bob" },
  { first: "Fred2", last: "bob2" },
];

const couple2: DancingDuo<{ first: string }> = [
  // {first:string} 에러 발생 ,  {first:string} 는 Name을 확장하지 않기 때문에 오류 발생
  { first: "sonny" },
  { first: "sonny2" },
];

//Type '{ first: string; }' does not satisfy the constraint 'Name'.
//Property 'last' is missing in type '{ first: string; }' but required in type 'Name'.(2344)
```

### 느낀점

- 코드의 중복을 줄이듯 타입의 중복도 줄여야 코드를 가독성있게 짤 수 있을 것이다.또한 표준라이브러리에 속하는 Partial이나 Pick등을 잘 활용하면 중복이나 코드의 양을 줄이는데 도움을 줄 수 있을 것 같다.

### 추가로 공유하고 싶은 내용

Partial 예제 (파셜 타입은 특정 타입의 부분 집합을 만족하는 타입을 정의할 수 있습니다)

```ts
interface Address {
  email: string;
  address: string;
}

type MyEmail = Partial<Address>;
const me: MyEmail = {}; // 가능
const you: MyEmail = { email: "noh5524@gmail.com" }; // 가능
const all: MyEmail = { email: "noh5524@gmail.com", address: "secho" }; // 가능
```

Pick (픽 타입은 특정 타입에서 몇 개의 속성을 선택하여 타입을 정의합니다)

```ts
interface Product {
  id: number;
  name: string;
  price: number;
  brand: string;
  stock: number;
}

// 상품 목록을 받아오기 위한 api
function fetchProduct(): Promise<Product[]> {
  // ... id, name, price, brand, stock 모두를 써야함
}

type shoppingItem = Pick<Product, "id" | "name" | "price">;

// 상품의 상세정보 (Product의 일부 속성만 가져온다)
function displayProductDetail(shoppingItem: shoppingItem) {
  // id, name, price의 일부만 사용 or 별도의 속성이 추가되는 경우가 있음
  // 인터페이스의 모양이 달라질 수 있음
}
```

Omit Pick의 반대

```ts
interface Product {
  id: number;
  name: string;
  price: number;
  brand: string;
  stock: number;
}

type shoppingItem = Omit<Product, "stock">;

const apple: Omit<Product, "stock"> = {
  id: 1,
  name: "red apple",
  price: 1000,
  brand: "del",
};
```
