# 1장 타입스크립트 알아보기

## 아이템2. 타입스크립트 설정 이해하기

### 읽은 내용

tsconfig.json파일에서 설정한다.(약 100가지 이상의 설정 가능)

다음 2가지가 중요한 설정이다.

- noImplicitAny : 변수들이 미리 정의된 타입을 가지는지 체크

```ts
fcuntion add(a,b) {  // a와 b에서 ts 오류 발생, 암시적으로 any형식이 포함됩니다.
	return a+b;
}

noImlicitAny설정시(true).

fcuntion add(a:number , b:number) {  // a와 b를 명시적으로 선언해주기
	return a+b;
}
```

- strictNullChecks : null과 undefined가 모든 타입에서 허용되는지 체크

```ts
const x: number = null; // strictNullChecks 해제 시 정상
const x: number = null; // strictNullChecks 설정 시 null형식은 number에 할당할 수 없다.
const x: number | null = null; // strictNullChecks 설징 시 명시적으로 타입을 표시하기

/**
strictNullChecks 설정시 
**/
const el = document.getElementById("status");

el.textContent = "Ready"; // 개체가 null인것 같다. 에러

if (el) {
  el.textContent = "Ready"; // 조건문을 통해 null체크를 해주어서 정상
}

el!.textContent = "Ready"; // null아님을 ! 를 통해서 단언
```

자바스크립트 프로젝트를 타입스크립트로 전환할때가 아니면 noImplicitAny설정 하기
"undefined"객체가 아닙니다 같은 런타임 오류 방지 하기 위해서 strictNUllChecks 설정 하기

### 느낀점

- tsconfig.json파일을 초기셋팅을 하고서는 건들지 않게 되는 경우가 많아 크게 신경쓰지 않았었는데, 중요한 기능을 하는 2가지 설정을 통해서
  타입스크립트를 좀 더 잘 쓸 수 있도록 하는 설정을 안 것 같다. 그동안 신경쓰지 않앗던 것들에 조금씩 다른 설정을 더 해봐야할것 같다.

### 추가로 공유하고 싶은 내용

tsconfig.json에서 쓸만한 내용들 정리

```json
{
  "compilerOptions": {
    "target": "es5", // 'es3', 'es5', 'es2015', 'es2016', 'es2017','es2018', 'esnext' 가능
    "module": "commonjs", //무슨 import 문법 쓸건지 'commonjs', 'amd', 'es2015', 'esnext'
    "allowJs": true, // js 파일들 ts에서 import해서 쓸 수 있는지
    "checkJs": true, // 일반 js 파일에서도 에러체크 여부
    "jsx": "preserve", // tsx 파일을 jsx로 어떻게 컴파일할 것인지 'preserve', 'react-native', 'react'
    "declaration": true, //컴파일시 .d.ts 파일도 자동으로 함께생성 (현재쓰는 모든 타입이 정의된 파일)
    "outFile": "./", //모든 ts파일을 js파일 하나로 컴파일해줌 (module이 none, amd, system일 때만 가능)
    "outDir": "./", //js파일 아웃풋 경로바꾸기
    "rootDir": "./", //루트경로 바꾸기 (js 파일 아웃풋 경로에 영향줌)
    "removeComments": true, //컴파일시 주석제거

    "strict": true, //strict 관련, noimplicit 어쩌구 관련 모드 전부 켜기
    "noImplicitAny": true, //any타입 금지 여부
    "strictNullChecks": true, //null, undefined 타입에 이상한 짓 할시 에러내기
    "strictFunctionTypes": true, //함수파라미터 타입체크 강하게
    "strictPropertyInitialization": true, //class constructor 작성시 타입체크 강하게
    "noImplicitThis": true, //this 키워드가 any 타입일 경우 에러내기
    "alwaysStrict": true, //자바스크립트 "use strict" 모드 켜기

    "noUnusedLocals": true, //쓰지않는 지역변수 있으면 에러내기
    "noUnusedParameters": true, //쓰지않는 파라미터 있으면 에러내기
    "noImplicitReturns": true, //함수에서 return 빼먹으면 에러내기
    "noFallthroughCasesInSwitch": true //switch문 이상하면 에러내기
  }
}
```
