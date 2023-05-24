# 2장 타입스크립트의 타입 시스템


## 아이템19. 추론 가능한 타입을 사용해 장황한 코드 방지하기

### 읽은 내용

> 모든 변수에 타입을 선언하는 것은 매우 비생상적이다.

`타입 추론` 이 가능하다면 명시적인 타입 선언은 필요하지 않다.  
그리고 복잡한 객체/배열인 경우에도 `타입 추론` 이 가능하다.

```ts
let x: number = 10;
let x = 12; // 타입 추론이 가능하다.

function square(nums: number[]) {
  return nums.map((x) => x * x);
}
const squres = square([1, 2, 3]) // number[]
```

비구조화 할당문은 모든 지역 변수의 타입이 추론된다.  
여기에 추가적인 명시적 타입 구문을 넣으시 불필요한 타입 선언으로 코드가 번잡해 질 수 있다.
```ts
function logProduct(product: Product) {
  // const id: number = product.id;
  // const name: string = product.name;
  const { id, name } = product;
}
```

타입스크립트는 변수와 타입이 일반적으로 처음 등장할 때 결정된다.  
하지만 라이브러리를 사용할 때 타입 정보가 기본적으로 제공된다면 콜백함수, 매개변수 타입은 자동으로 추론된다.

타입을 명시해야 되는 상황이 있는데, 그것은 객체 리터럴을 정의할 때이다.  
잘못된 데이터 반영이나 변수 이름 오타 등으로 발생하는 문제를 방지하기 위해 명시적으로 타입을 선언하는 것이 좋다.

```ts
interface Product {
  name: string;
  id: number;
  price: number;
}

const product: Product = {
  name: 'bytrustu',
  id: '1234',  // 오류 발생.
  price: 1000,
}
```

---

## 아이템20. 다른 타입에는 다른 변수 사용하기

### 읽은 내용

타입스크립트의 변수의 값은 바뀔 수 있지만 타입은 바뀌지 않는다.  

타입이 다른 값을 다룰 때에는 변수를 재사용하지 말고, 서로 관련이 없는 두 개의 값을 분리하자.  
그러면 타입 추론이 향상되고, 타입 구분이 불필요해진다.  

```ts
// 👎 Bad Case
let id: string | number = '12-34-56';
fetchProduct(id); 

id = 123456;
fetchProductBySerialNumber(id);

// 👍 Good Case
let id = '12-34-56';
fetchProduct(id); 

let serial = 123456;
fetchProductBySerialNumber(serial);
```

---

## 아이템21. 타입 넓히기

### 읽은 내용

타입스크립트는 타입 체크를 하는 컴파일 시점에 변수는 가능한 값들의 집한인 `타입` 을 가진다.

> ### 타입 넓히기 란?
> 타입스크립트에서의 과정으로 지정된 단일 값을 가지고 할당 가능한 값들의 집합을 유추해야 한다.  
> 즉, 변수나 식별자의 타입이 더 넓은 범위로 확장되는 것을 의미하고,  
> `타입 추론 과정에서 타입이 자동으로 확장` 될 때 발생한다.

상수를 사용해서 변수를 초기화할 경우, 타입을 명시하지 않으면 타입 체커가 타입을 결정하게 된다.  
할당 가능한 값을 유추해서 결정한다.  

`타입 넓히기` 가 진행될 때 주어진 값을 추론 가능한 타입이 여러개이기 때문에 과정이 상당히 모호하다.  

타입을 넓히는 과정을 제어하는 방법으로 몇가지가 있다.

### 📖 const를 변수를 선언한다.  
const를 변수를 선언하면 재할당이 불가능하기 때문에 타입스크립트는 좁은 타입으로 추론한다.

```ts
const x = 'x'; // 타입은 'x'
```

### 📖 명시적 타입 구문을 제공한다.
```ts
const value: { x: 1 | 2 | 3 } = {
  x: 1,
};
```

### 📖 타입체커에 추가적인 문맥을 제공한다.

`as const` 를 작성하면, 타입스크립트는 최대한 좁은 타입으로 추론한다.

```ts
const v1 = {
  x: 1,
  y: 2,
}; // 타입은 { x: number; y: number; }

const v2 = {
  x: 1 as const,
  y: 2,
}; // 타입은 { x: 1; y: number; }

const v3 = {
  x: 1,
  y: 2,
} as const; // 타입은 { readonly x: 1; readonly y: 2; }
```

---