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
