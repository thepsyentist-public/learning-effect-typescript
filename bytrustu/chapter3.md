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


