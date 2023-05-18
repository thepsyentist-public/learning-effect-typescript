# 2장 타입스크립트의 타입 시스템

## 아이템18. 매핑된 타입을 사용하여 값을 동기화하기

### 읽은 내용

데이터나 디스플레이 속성이 변경되면 다시 그려야하지만, 이벤트 핸들러가 변경되면 다시 그릴 필요가 없다는 것에 대한 예제

```ts
interface ScatterProps {
    xs: number[];
    ys:number[];

    xRange: [number,number];
    yRange: [number,number];
    color:string;

    onClick: (x:number, y:number, index:number) => void;
}


const REQUIRES_UPDATE: {[k in keyof ScatterProps]:boolean} = {  // 타입체커에게 REQUIRES_UPDATE가 ScatterProps와 동일한 속성을 가져야한다는것을 제공
    xs: true,
    ys: tr
    xRange: true,
    yRange: true,
    color:true,
    onClick:false
}

function shouldUpdate(oldProps:ScatterProps, newProps:ScatterProps) {
    let k: keyof ScatterProps;
    for(k in oldProps) {
        if(oldProps[k] !== newProps[k] && REQUIRES_UPDATE[k]) {
            return true;
        }
    }
    return false
}
```

매핑된 타입을 사용하여 관련된 값과 타입을 동기화할것!
어떤 새로운 속성을 추가할때에도 선택을 강제할 수 있도록(오류를 줄일 수 있도록) 매핑된 타입을 사용하자

### 느낀점

- 매핑된 타입을 사용하여 내가 결과에 대해 미리 타입을 지정하게되면, 실수를 방지할 수 있을것 같다.

### 추가로 공유하고 싶은 내용

매핑타입 추가 정보

TypeScript의 매핑 타입은 기존 타입을 변형하여 새로운 타입을 생성하는 기능을 제공합니다. 매핑 타입은 제네릭 타입과 조건부 타입을 이용하여 구현됩니다.

가장 일반적인 매핑 타입은 Record 타입입니다. Record<K, V>는 키 타입 K와 값 타입 V를 매핑하여 새로운 객체 타입을 생성합니다.

예를 들어, Record<string, number>는 문자열 키를 가지고 숫자 값을 가지는 객체를 나타냅니다.

또 다른 유용한 매핑 타입은 Partial과 Readonly입니다.
Partial<T>는 타입 T의 모든 속성을 선택적으로 만들어줍니다. Readonly<T>는 타입 T의 모든 속성을 읽기 전용으로 만들어줍니다.

매핑 타입은 keyof 연산자와 함께 사용하여 타입의 특정 속성을 추출하거나 조작할 수도 있습니다.
예를 들어, keyof T는 타입 T의 모든 속성 키를 추출합니다.(위 예제)
