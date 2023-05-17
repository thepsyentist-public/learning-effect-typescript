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

---

## 추가1. 타입스크립트 tsconfig 설정
```json
{
    "compilerOptions": {
        // 옵션 형식으로 구성되어 있습니다.
        // "모듈 키": 모듈 값                        /* 설명: 사용가능 옵션 (설명이 "~ 여부"인 경우 'true', 'false') */

        /* 기본 옵션 */
        // "incremental": true,                   /* 증분 컴파일 설정 여부 */
        "target": "es5" /* 사용할 특정 ECMAScript 버전 설정: 'ES3' (기본), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', 혹은 'ESNEXT'. */,
        "module": "commonjs" /* 모듈 불러오는 코드 방식 설정: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
        // "lib": [],                             /* 컴파일에 포함될 라이브러리 파일 목록 */
        // "allowJs": true,                       /* 자바스크립트 파일 컴파일 허용 여부 */
        // "checkJs": true,                       /* .js 파일의 오류 검사 여부 */
        // "jsx": "preserve",                     /* JSX 코드 생성 설정: 'preserve', 'react-native', 혹은 'react'. */
        // "declaration": true,                   /* '.d.ts' 파일 생성 여부. */
        // "declarationMap": true,                /* 각 '.d.ts' 파일의 소스맵 생성 여부. */
        // "sourceMap": true,                     /* '.map' 파일 생성 여부. */
        // "outFile": "./",                       /* 단일 파일로 합쳐서 출력합니다. */
        // "outDir": "./",                        /* 해당 디렉토리로 결과 구조를 보냅니다. */
        // "rootDir": "./",                       /* 입력 파일의 루트 디렉토리(rootDir) 설정으로 --outDir로 결과 디렉토리 구조를 조작할 때 사용됩니다. */
        // "composite": true,                     /* 프로젝트 컴파일 여부 */
        // "tsBuildInfoFile": "./",               /* 증분 컴파일 정보를 저장할 파일 */
        // "removeComments": true,                /* 주석 삭제 여부 */
        // "noEmit": true,                        /* 결과 파일 내보낼지 여부 */
        // "importHelpers": true,                 /* 'tslib'에서 헬퍼를 가져올 지 여부 */
        // "downlevelIteration": true,            /* 타겟이 'ES5', 'ES3'일 때에도 'for-of', spread 그리고 destructuring 문법 모두 지원 */
        // "isolatedModules": true,               /* 각 파일을 분리된 모듈로 트랜스파일 ('ts.transpileModule'과 비슷합니다). */

        /* 엄격한 타입-확인 옵션 */
        "strict": true /* 모든 엄격한 타입-체킹 옵션 활성화 여부 */,
        // "noImplicitAny": true,                 /* 'any' 타입으로 구현된 표현식 혹은 정의 에러처리 여부 */
        // "strictNullChecks": true,              /* 엄격한 null 확인 여부 */
        // "strictFunctionTypes": true,           /* 함수 타입에 대한 엄격한 확인 여부 */
        // "strictBindCallApply": true,           /* 함수에 엄격한 'bind', 'call' 그리고 'apply' 메소드 사용 여부 */
        // "strictPropertyInitialization": true,  /* 클래스의 값 초기화에 엄격한 확인 여부 */
        // "noImplicitThis": true,                /* 'any' 타입으로 구현된 'this' 표현식 에러처리 여부 */
        // "alwaysStrict": true,                  /* strict mode로 분석하고 모든 소스 파일에 "use strict"를 추가할 지 여부 */

        /* 추가적인 확인 */
        // "noUnusedLocals": true,                /* 사용되지 않은 지역 변수에 대한 에러보고 여부 */
        // "noUnusedParameters": true,            /* 사용되지 않은 파라미터에 대한 에러보고 여부 */
        // "noImplicitReturns": true,             /* 함수에서 코드의 모든 경로가 값을 반환하지 않을 시 에러보고 여부 */
        // "noFallthroughCasesInSwitch": true,    /* switch문에서 fallthrough 케이스에 대한 에러보고 여부 */

        /* 모듈 해석 옵션 */
        // "moduleResolution": "node",            /* 모듈 해석 방법 설정: 'node' (Node.js) 혹은 'classic' (TypeScript pre-1.6). */
        // "baseUrl": "./",                       /* non-absolute한 모듈 이름을 처리할 기준 디렉토리 */
        // "paths": {},                           /* 'baseUrl'를 기준으로 불러올 모듈의 위치를 재지정하는 엔트리 시리즈 */
        // "rootDirs": [],                        /* 결합된 컨텐츠가 런타임에서의 프로젝트 구조를 나타내는 루트 폴더들의 목록 */
        // "typeRoots": [],                       /* 타입 정의를 포함할 폴더 목록, 설정 안 할 시 기본적으로 ./node_modules/@types로 설정 */
        // "types": [],                           /* 컴파일중 포함될 타입 정의 파일 목록 */
        // "allowSyntheticDefaultImports": true,  /* default export이 아닌 모듈에서도 default import가 가능하게 할 지 여부, 해당 설정은 코드 추출에 영향은 주지 않고, 타입확인에만 영향을 줍니다. */
        "esModuleInterop": true /* 모든 imports에 대한 namespace 생성을 통해 CommonJS와 ES Modules 간의 상호 운용성이 생기게할 지 여부,  'allowSyntheticDefaultImports'를 암시적으로 승인합니다. */,
        // "preserveSymlinks": true,              /* symlik의 실제 경로를 처리하지 않을 지 여부 */
        // "allowUmdGlobalAccess": true,          /* UMD 전역을 모듈에서 접근할 수 있는 지 여부 */

        /* 소스 맵 옵션 */
        // "sourceRoot": "",                      /* 소스 위치 대신 디버거가 알아야 할 TypeScript 파일이 위치할 곳 */
        // "mapRoot": "",                         /* 생성된 위치 대신 디버거가 알아야 할 맵 파일이 위치할 곳 */
        // "inlineSourceMap": true,               /* 분리된 파일을 가지고 있는 대신, 단일 파일을 소스 맵과 가지고 있을 지 여부 */
        // "inlineSources": true,                 /* 소스맵과 나란히 소스를 단일 파일로 내보낼 지 여부, '--inlineSourceMap' 혹은 '--sourceMap'가 설정되어 있어야 한다. */

        /* 실험적 옵션 */
        // "experimentalDecorators": true,        /* ES7의 decorators에 대한 실험적 지원 여부 */
        // "emitDecoratorMetadata": true,         /* decorator를 위한 타입 메타데이터를 내보내는 것에 대한 실험적 지원 여부 */

        /* 추가적 옵션 */
        "skipLibCheck": true /* 정의 파일의 타입 확인을 건너 뛸 지 여부 */,
        "forceConsistentCasingInFileNames": true /* 같은 파일에 대한 일관되지 않은 참조를 허용하지 않을 지 여부 */
    }
}
```

---

## 추가2. 타입스크립트의 컴파일 과정

타입스크립트의 컴파일 과정으로는 `타입 검사` `트랜스파일` 과정이 있습니다.

`타입 검사` 단계에서는 타입스크립트 코드를 분석하고, 코드에 대한 타입 정보를 수집합니다.  
이를 통해서 타입 관련한 오류를 미리 방지하게 됩니다.

`트랜스파일` 단계에서는 타입 검사를 통과한 코드를 자바스크립트 코드로 변환합니다.  
이 과정은 `문법 변환` `폴리필 삽입` 과정으로 진행 됩니다.

`문법 변환` 은 **추상 구문 트리(AST)** 로 변환이 되고, 다시 자바스크립트 코드로 변환됩니다.  
이 과정에서 타입의 정보는 제거되고, 이전 버전의 브라우저에서도 실행 가능하도록 코드가 반영됩니다.

`폴리필 삽입` 은 자바스크립트의 최신 기능이 브라우저에서 지원되지 않은 경우에 코드를 삽입하는 역할을 합니다.  
예를 들어 ES6 이후의 스펙인 경우 Promise는 ES5 이하에서 지원하지 않지만 폴리필을 통해서 지원할 수 있습니다.  
주로 **core-js**, **regenerator-runtime** 라이브러리가 활용됩니다.


## 추가3. 구조적 타이핑 / 명시적 타이핑

[링크](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgOIRNYCCS5rxLIDeAUMsgNoAeAXMgM5hSgDmAuvSAK4C2ARtADcpAL6kwATwAOKAHIB7KLzgAbACoyUAXhLI6yHgOjJRI5KQQKQTQ0pUat9RcrWbZyXcX30AjKZErGzBkVgwsXHxYRAh6dEwWSMhool0QezctIWQAehy7ZGgoJVIJLTRwxPcdPRp6JhYQDi4+QSgA0lBkwhR011U8bpiSch9DVuExcktrWz6HQYIYrgyBqJ7PPQN-Mxng0MrsarjDhGrN+bVFlIhsvMKoYqggA)  
위 링크 코드를 통해, 인터페이스의 구조적 타이핑과 타입의 명시적 타이핑에 대한 차이를 이해해볼수 있습니다.
