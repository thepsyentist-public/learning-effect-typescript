# 3장 타입스크립트의 타입 시스템

## 아이템19. 추론 가능한 타입을 사용해 장황한 코드 방지하기

### 읽은 내용

```ts
let x: number = 12; // 이렇게 써도 되긴하지만

let x = 12; // 이렇게 써도 x는 number로 추론된다

const person: {
  name: string;
  born: {
    where: string;
    when: string;
  };
  died: {
    where: string;
    when: string;
  };
} = {
  name: "name",
  born: {
    where: "bornwher",
    when: "bornwhen",
  },
  died: {
    where: "diedwhere",
    when: "diedwhen",
  },
};

const person = {
  name: "name",
  born: {
    where: "bornwher",
    when: "bornwhen",
  },
  died: {
    where: "diedwhere",
    when: "diedwhen",
  },
}; // 객체 또한 동일하게 타입을 지정하지 않아도 추론된다
```

타입 구문을 제대로 명시할떄와 제거할때

```ts
interface Product {
  id: string;
  name: string;
  price: number;
}

function logProduct(product: Product) {
  const { id, name, price } = product;
  console.log(id, name, price);
}

// 타입 구문을 제거하였을때
const furby = {
  name: "a",
  id: 1234,
  price: 100,
};

logProduct(furby); // 에러발생, 객체를 선언한곳이아니라 사용되는곳에서 오류가 발생!

// Argument of type '{ name: string; id: number; price: number; }' is not assignable to parameter of type 'Product'.
//   Types of property 'id' are incompatible.
//     Type 'number' is not assignable to type 'string'

//타입 구문을 제대로 명시했을때 , 실수가 발생한 부분에 오류 발생
const furby: Product = {
  name: "a",
  id: 1234, // 에러발생 Type 'number' is not assignable to type 'string'.(2322)
  price: 100,
};

logProduct(furby);
```

함수도 반환타입 명시하기!

```ts
function getQuote(ticker: string) {
  return fetch(`https://quotes.example.com/?1=${ticker}`).then((response) =>
    response.json()
  );
}

const cache: { [ticker: string]: number } = {};
function getQuote(ticker: string): Promise<number> {
  // 함수 반환 타입을 명시하여야 오류의 표시위치가 정확하게 어디인지 알 수 있다.
  if (ticker in cache) {
    return cache[ticker]; // 해당부분에러 발생
  }

  return fetch(`https://quotes.example.com/?1=${ticker}`)
    .then((response) => response.json())
    .then((quote) => {
      cache[ticker] = quote;
      return quote;
    });
}
```

> 반환타입을 명시해야하는 이유

- 함수에 대해 더욱 명확하게 할 수 있다.
- 명명된 타입을 사용하기 위해서 이다.

### 느낀점

- 타입스크립트가 나름 똑똑하여 타입을 명명하지 않아도 추론하는 경우가 있는데, 이것을 활용하여 불필요한 타입선언을 줄일 수 있다는 것을 알았다. 하지만, 함수 반환타입을 명시하지 않는 경우 오류의 위치를 정확하게 알 수 없기때문에 함수 반환타입 명시는 하는게 좋겠다.
