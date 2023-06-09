# 5장 any 다루기

## 아이템43. 몽키 패치보다는 안전한 타입 사용하기

### 읽은 내용

```ts
document.monkey = "AAA";

(document as any).monkey = "BBB";
```

any를 사용하면 객체에 임의로 추가한 속성에 대해 에러가 발생하지 않는다.
하지만, 타입 안정성을 상실하고 언어 서비스를 사용할 수 없다.

> 해결책1. 보강을 사용하기

```ts
interface Document {
    monkey: string;
}

document.monkey = "AAA";
```

모듈 관점에서 global하게 사용하려면

```ts
export {};

declare global {
    interface Document {
        monkey: string;
    }
}

document.monkey = "AAA";
```

> 해결책2. 더 구체적인 타입 단언문 사용하기

```ts
interface MonkeyDocumnet extends Document {
    monkey: string;
}

(document as MonkeyDocumnet).monkey = "AAA";
```
