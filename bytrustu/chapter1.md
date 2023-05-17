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
