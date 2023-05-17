# 1장 타입스크립트 알아보기


## 아이템1. 타입스크립트와 자바스크립트의 관계 이해하기

### 읽은 내용

타입스크립트는 결국 자바스크립트가 만들어내는 도구이다.  
정적 타입은 유효성 체크일뿐 타입이 맞지 않다고해서 그것이 자바스크립트로써 동작하지 않는다는 의미가 아니다.  

타입스크립트의 목표 중 하나는 `런타임에 발생시킬 오류`를 미리 찾아내는 것이다.  
타입스크립트는 어느쪽이 오타인지 판단하지 못한다.  

```ts
const states = [
  { name: 'Alabama', capitol: 'Montgomery' },
  { name: 'Alaska', capitol: 'Juneau' },
  { name: 'Arizona', capitol: 'Phoenix' },
];

for (const state of states) {
  console.log(state.capital); // Property 'capital' does not exist on type '{ name: string; capitol: string; }'. Did you mean 'capitol'?
}
```

따라서 명시적으로 타입을 선언해주어야 한다.
```ts
interface State {
  name: string;
  capital: string;
}

const states: State[] = [
  { name: 'Alabama', capital: 'Montgomery' },
  { name: 'Alaska', capitol: 'Juneau' },
  { name: 'Arizona', capital: 'Phoenix' },
];
```

### 느낀점
- 타입스크립트는 자바스크립트를 기반으로 정적 타입 시스템을 추가한 언어라는 것을 알게 되었다.
- 정적 타입 시스템은 변수나 함수의 타입을 명시적으로 지정해서 컴파일 타임에서 오류가 발생하는데, 이를 통해 자바스크립트의 동적 타입에서 있을수 있는 오류들을 사전에 파악하고, 모호할 수 있는 부분들을 개선할 수 있다는 점을 배웠다.  

---

## 아이템2. 타입스크립트 설정 이해하기

### 읽은 내용

타입스크립트는 설정을 어떻게 하느냐에 따라 완전히 다른 언어처럼 느껴질 수 있고, 제대로 사용하기 위해선 tsconfig의 옵션인 `noImplicitAny` `strictNullCheckes`를 이해하는 것이 중요하다.

> ### noImplictAny
> 함수의 매개변수나 반환값에 타입이 명시되지 않은 경우 컴파일러가 경고 메세지를 출력하는 옵션이다.
이 옵션이 설정되어 있지 않은 경우, 매개변수와 반환값의 타입을 암묵적으로 any타입으로 지정하여 코드에 타입 관련 오류가 있어도 컴파일이 되므로, 런타임 시 에러가 발생할 가능성이 있다.

> ### stictNullChecked
> 변수, 매개변수, 속성등의 값이 null, undefined일 수 있다면, 그 값을 사용할 때 명시적으로 null, undefined를 처리하도록 강제한다.

### 느낀점 
- 타입스크립트를 어떻게 설정하느냐에 따라 프로젝트에서 사용하는 타입스크립트의 특성이 달라질 수 있다는 것을 알게 되었다.

---

## 아이템3. 코드 생성과 타입이 관계없음을 이해하기

### 읽은 내용

> **타입스크립트는 컴파일러로써 두가지 역할을 한다.**
1. 최신 타입스크립트/자바스크립트를 브라우저에서 구동되도록 구버전 JS로 트랜스파일 한다.
2. 코드의 타입 오류를 체크한다.

<br />

> **타입스크립트는 타입 오류가 있는 코드도 컴파일이 가능하다.**  

Rectangle은 타입으로서 존재하기 때문에 런타임 시점에서는 지워진다. 즉 값으로는 사용될 수 없다.

```ts
interface Square {
  width: number;
}

interface Rectangle extends Square {
  height: number;
}

type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    return shape.width * shape.height;
  } else {
    return shape.width * shape.width;
  }
}
```

위 문제를 해결하기 위해서는 태그 된 유니온 기법을 사용한다.
```ts
interface Square {
  kind: 'square';
  width: number;
}
interface Rectangle {
  kind: 'rectangle';
  width: number;
  height: number;
}

type Shape = Sqaure | Rectangle;

function calculateArea(shape: Shape) {
  if (shape.kind === 'rectangle') {
    return shape.width * shape.height;
  } else {
    return shape.width * shape.width;
  }
}
```
<br />

> **타입 연산은 런타임에 영향을 주지 않는다.**

타입단언(as)은 타입의 연산이고, 런타임 동작에 아무런 영향을 주지 않는다.  
값을 정제하기 위해서는 런타임에 타입을 체크해야하고 자바스크립트 연산을 통해 변환을 수행해야 한다.  


### 느낀점 
타입스크립트는 자바스크립트의 런타임에서 있을수 있는 오류를 사전에 방지하는 것이 핵심 목적이다.  
그리고 객체간 타입을 정의하는 것에 있어서 그 관계를 알고 명시적으로 작성해야겠다.

---

## 아이템4. 구조적 타이핑에 익숙해지기

### 읽은 내용

`덕 타이핑(Duck Typing)`은 객체가 어떤 타입에 걸맞는 변수와 메소드를 지니면 객체를 해당 타입에 속하는것으로 간주한다.  
>“만약 어떤 새가 오리처럼 걷고, 헤엄치고, 소리를 낸다면 나는 그 새를 오리라고 부를 것이다.”

타입스크립트는 타입 검사를 `컴파일` 단계에서 진행한다.  
타입 검사 측면에서 타입스크립트를 바라보면 타입스크립트는 `정적 타이핑` 이라고 말할 수 있다.  
Duck Typing은 타입스크립트의 동적타이핑으로 다형성의 측면에서 해석한 결과라고 할 수 있다.

### 느낀점

인터페이스의 구조적 타이핑과 타입의 명시적 타이핑을 구분할 필요성을 느꼈습니다.


---

## 아이템5. any 타입 지양하기


### 읽은 내용

타입스크립트에서 any 타입을 지양해야하는 이유는 `타입의 안전성`을 보장할 수 없고, `타입 추론`과 `타입체커`가 제대로 작동하지 않기 떄문이다.  
타입스크립트를 씀으로써 `타입에 대한 신뢰도`를 바탕으로 개발을 진행 할 수 있지만, 그에 대한 효과를 볼 수 없다.  
그리고 버그를 감추는 결과를 만들 수 있다. 

### 느낀점

타입스크립트를 쓰는 이유에 대해서 다시 한번 생각해보는 계기가 되었습니다. 
결국 개발을 진행하면서 타입을 명시하고 그것을 추론해서 유지보수 적인 측면까지 고려할 수 있는데,  
코어한 로직에 `any` 타입을 명시하게 되면, 이후에 있을 이슈들을 예측할 수 없고, 고려한 측면을 무시하게 되는 것 같습니다.  

