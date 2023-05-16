# 1장 타입스크립트 알아보기

## 아이템1. 타입스크립트와 자바스크립트의 관계 이해하기

### 읽은 내용

타입스크립트는 자바스크립트의 super set(상위 집합)이다.

타입스크립트의 목표중 하나는 런타임에 오류를 발생시킬 코드를 미리 찾아내는 것

but, 예외도 존재

```js
const states = [
  { name: "Alabama", capital: "Montgomery" },
  { name: "Alaska", capital: "Juneau" },
  { name: "Arizona", capital: "Phoenix" },
];

for (const state of states) {
  console.log(state.capitol); // 자바스크립트의 경우 undefined 를 출력한다.
  // 반면 타입스크트는 오류를 찾아내지만, 어느 쪽이 오타인지까지는 찾아낼 수 는 없다. (capitol vs capital)
}
```

명시적으로 타입을 선언하여 의도를 분명하게 하는게 좋다.

```ts
interface State {
  name: string;
  capital: string;
}

const states: State[] = [
  { name: "Alabama", capital: "Montgomery" },
  { name: "Alaska", capitol: "Juneau" },
  { name: "Arizona", capital: "Phoenix" },
];
```

타입스크립트는 자바스크립트 런타임 동작을 모델링하는 타입 시스템을 가지고 있기 때문에 런타임 오류를 발생시키는 코드를 찾아내려고 한다.
but, 타입 체커를 통과하면서도 런타임 오류를 발생시키는 코드는 충분히 존재할 수 있다.

```ts
const names = ["Alice", "bob"];

console.log(names[2].toUpperCase()); // TypeError: Cannot read property 'toUpperCase' of undefined
```

### 느낀점

- 타입스크립트는 자바스크립트의 슈퍼셋이다라는 말을 많이 들었지만, 정확히 어떤 의미인지는 몰랐던 부분을 조금 자세히 알게되었다.
- 타입스크립트가 이해하는 값과 실제 값에 차이가 존재할 수 있기때문에 타입스크립트를 사용할때에는 내가 의도한 것이 맞는지에 검증과정이 필요할 것 같다는 생각을 하였다.

### 추가로 공유하고 싶은 내용

런타임과 컴파일타임
컴파일 : 개발자가 작성한 소스코드를 컴파일 하여 기계어로 변환하는 과정
런타임 : 컴파일 마친 프로그램이 사용자에 의해 실행되어 동작 되어지는 때

자바스크립트의 런타임은 자바스크립트 엔진, Web API, 콜백 큐, 이벤트 루프, 렌더 큐로 구성
컴퍼일타임 에러 : 문법에러, 파일 참조 오류, 타입 체크 오류 등
런타임 에러 : 0으로 나누기 오류, Null 참조 오류, 메모리 부족 오류 등
