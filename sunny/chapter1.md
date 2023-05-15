# 1장 타입스크립트 알아보기


## 아이템1. 타입스크립트와 자바스크립트의 관계 이해하기

### 읽은 내용

타입스크립트는 결국 자바스크립트가 만들어내는 도구이다.  
정적 타입은 유효성 체크일뿐 타입이 맞지 않다고해서 그것이 자바스크립트로써 동작하지 않는다는 의미가 아니다.  

타입스크립트의 목표 중 하나는 런타임에 발생시킬 오류를 미리 찾아내는 것이다.  
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
