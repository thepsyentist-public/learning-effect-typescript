# 2장 타입스크립트의 타입 시스템


## 아이템6. 편집기를 사용하여 타입 시스템 탐색하기

### 읽은 내용

타입 스크립트를 설치하면 두 가지를 실행할 수 있다.  
- 타입스크립트 컴파일러(tsc)
- 단독으로 실행할 수 있는 타입스크립트 서버(tsserver)

타입스크립트는 컴파일러 실행 목적이지만, `언어 서비스`를 제공한다.
> **언어서비스란?**  
> 코드 자동완성, 검색, 명세검사가 가능하고 타입 추론을 가능해주게 한다.

반환 타입을 정해주지 않아도 인자의 타입을 추론하여 반환타입을 추론해준다.  
```ts
const num = (a: number, b: number) => a + b;
```

라이브러리와 라이브러리 타입 선언을 탐색할 때에도 도움을 준다.  
fetch에서 정의로 이동 옵션을 누르면 타입스크립트에 포함되어 있는 DOM타입 선언인 lib.dom.ts로 이동한다.
```ts
const response = fetch('https://test.com');
```

### 추가로 공유하고 싶은 내용
리액트에서도 컴포넌트의 Props 타입을 지정할때, HTML 태그 타입을 활용가능하다.  
```ts
// BAD: 인덱스 시그니처 타입은 탈출구 같은 느낌으로 사용된다.
type Props =  {
    className?: string;
    color?: ButtonColor;
    [key: string]: any;
}

// GOOD: HTML 태그를 사용하면 타입을 명시적으로 활용가능하다. 언어서비스도 지원된다.
type ButtonProps = {
    className?: string;
    color?: ButtonColor;
}
type Props = ButtonPrpos & HTMLAttributes<HTMLButtonElement>;

const Button = ({ color = 'primary', ...props }: Props) => (
    ...
);
```

---

## 아이템7. 타입이 값들의 집합이라고 생각하기

### 읽은 내용  

**할당 가능한 값들의 집합**을 `타입`이라고 한다.  
그리고 이러한 집합들은 타입의 범위라고 부르기도 한다.  

**가장 작은 집합**은 아무 값도 포함되지 않는 공집합으로 타입스크립트에서는 `never`이다. 값의 할당이 불가능하다.  
반대로 **가장 큰 집합**은 `unknown`이다. 어떠한 값이든 할당할 수 있는 집합이다.

<img src="https://github.com/thepsyentist-public/learning-effect-typescript/assets/39726717/a9fa8868-e422-40f4-9aa1-2c5925b20233" />


> ### unknown과 any의 차이점은 무엇일까?
> unkown은 어떤 값이든 할당할 수 있다. 하지만 이 값을 사용하기 위해서는 해당 값이 어떤 타입인지 미지 알아야한다.  
> 그래서 사용할 때는 보통 타입 검사를 통해 해당 값이 안전하게 사용될 수 있는지 확인 절차를 거친다.  
> any는 컴파일러가 타입 검사를 건너뛰고 해당 값에 대한 타입 체킹을 하지 않는다.  
> 서로의 차이점은 `컴파일러의 타입 체크 여부`에 해당한다.

타입스크립트에는 집합을 나타내는 몇가지 타입이 있다.
- **유닛(unit) 타입**: 한 가지 값만 포함하는 타입
- **유니온(union) 타입**: 두 개 혹은 세 개로 묶으려는 타입, 합집합, | 연산자
- **인터섹션(intersection) 타입**: 두 타입에서 둘 다 가지는 공통 속성 타입, 교집합, & 연산자

그리고 부분집합의 의미로는 **extends**를 사용할 수 있다.  
extends는 ~에 할당가능한 이란 뜻으로 ~의 부분집합이라는 의미를 가진다.

```ts
interface Person {
  name: string;
}

interface Information extends Person {
  id: number;
  password: string;
}
```

### 느낀점
타입스크립트는 타입을 집합으로 생각하고, 집합의 관계를 통해 타입을 이해하는 것이 중요하다.  
타입을 사용하는 과정에서 동적으로 변경될 수 있는 경우라면, any 보다는 unkown을 통해서 `타입캐스팅`을 이용하는것이 좋을것 같다.


---

## 아이템8. 타입공간과 값 공간의 심벌 구분하기

### 읽은 내용

타입스크립트의 심벌은 `타입 공간` `값 공간` 중의 한곳에 존재한다.

- 타입
    - type, interface 키워드를 사용한다.
    - `타입선언(:)` `타입단언(as)` 다음에 사용한다.
- 값
    - const, let 키워드를 사용한다.
    - =(할당) 키워드 다음에 사용한다.

`class` `enum` 은 값과 타입을 모두 가능한 예약어다.
```ts
class Cylinder {
  radius = 1;
  height = 2;
}

function calcVolumne(shape: unkown) {
  if (shape instanceof Cylinder) {
    shapre.radius; // 에러가 발생하지 않는다. 값의 공간으로 사용된다.
  }
}
```

---

## 아이템9. 타입 단언보단 타입선언 이용하기

### 읽은 내용

타입 단언은 강제로 그 타입을 주는 것이라서 실제 타입과 다르더라도 오류를 표시하지 않는다.  
`타입스라이팅`이라서 해도 좋겠다.

```ts
interface Person {
  name: string;
}

// Person에 occupation속성이 없으므로 오류가 발생한다.
const alice: Person = {
  name: 'Alice',
  occupation: 'Typescript developer',
}

// 오류가 발생하지 않는다.
const bob = {
  name: 'Bob',
  occupation: 'Javascript developer',
} as Person;
```

> ### 타입 단언은 언제 사용하면 좋을까?
> 타입 단언은 `타입 추론(Type Interface)` 이 제대로 이루어지지 않을때 사용할 수 있다.  
> 타입스크립트는 DOM에 접근할 수 없기 때문에 컴파일 과정에서 DOM 요소의 타입을 정확히 알아내기 어렵다.  
> 이런 경우에는 타입 단언을 사용하여 DOM 요소의 타입을 명시적으로 지정해주는 것이 필요하다.


---

## 아이템10. 객체 래퍼 타입 피하기

### 읽은 내용

자바스크립트의 기본형은 number, string, null, boolean, symbol, bigint 로 불변적 성격을 가지며, 메소드를 가지고 있지 않다.  
하지만 타입마다 래퍼타입을 가지고 있다.

```js
'string'.chatAt(2);
// "r"
```

string에는 메서드가 없지만 자바스크립트에서는 기본형과 객체 타입을 서로 자유롭게 변환이 가능하다.  
`기본형` → `객체형으로 변환` → `메소드 호출` → `래핑한 객체 버림` 순서로 진행하여 메서도를 사용하는 것처럼 보이게 된다.  

객체형과 기본형이 서로 변환이 쉽지만 객체형은 오직 자기자신하고만 동일하다.  
그리고 객체래퍼의 타입변환은 어떤 속성을 기본형에 할당하면 그 속성은 사라진다.

타입스크립트는 기본형과 객체 래퍼타입을 별도로 모델링하므로 객체 래퍼는 필요할 때 제외하고는 지양하자.

---

## 아이템11. 잉여속성체크의 한계 인지하기

### 읽은 내용

타입이 명시 된 변수에 리터럴을 할당할 때에는 구조적 타이핑이 무력화 됩니다.  
이와 같은 동작을 잉여 속성 체크라고 합니다.  

```ts
interface Room {
  numDorrs: number;
  ceilingHeightFt: number;
}

const r: Room = {
  numDorrs: 1,
  ceilingHeightFt: 10,
  elephant: 'present' // 오류 발생
}

const obj = {
  numDorrs: 1,
  ceilingHeightFt: 10,
  elephant: 'present',
};

const r2: Room = obj; // 정상, 임시 변수를 도입하면 잉여 속성 체크가 동작하지 않는다.
```

임의로 변수를 이용해서 할당을 하면 잉여 속성 체크가 작동하지 않는다.  
이것이 허용되는 이유는 `잉여 속성 체크`가 **할당 가능 검사와는 별도의 과정**이기 때문이다.  
잉여 속성 체크는 엄격한 객체 리터럴 체크라고 하며, 객체 리터럴을 사용하지 않는 할당이나 타입 단언문을 사용할 때에도 적용되지 않는다.

---

## 아이템12. 함수 표현식에 타입 적용하기

### 읽은 내용

함수 표현식을 사용하면 매개변수와 반환 값을 한번에 선언할 수 있다.  
반면에 함수 선언문을 사용하면 함수의 매개변수와 반환 값의 타입을 따로 선언해야한다.  
**재사용성**을 고려해서 `함수 표현식`으로 작성하도록 하자.

```ts
// 함수 선언문
function add(a: number, b: number): number { return a + b }
function sub(a: number, b: number): number { return a - b }

// 함수 표현식
type CalculatorType = (a: number, b: number) => number;
const add: CalculatorType = (a, b) => a + b;
const sub: CalculatorType = (a, b) => a - b;
```
