# 4장 타입스크립트의 타입 시스템

## 아이템33. string타입보다 더 구체적인 타입 사용하기

### 읽은 내용

string을 남발하지 않기

```ts
interface Album {
    artist: string;
    title: string;
    releaseDate: string;
    recordingType: string;
}

// 타입을 명확하게 표시하는게 좋다. 날짜는 날짜로, 타입은 타입별로

type RecordingType = "studio" | "live";

interface Album {
    artist: string;
    title: string;
    releaseDate: Date;
    recordingType: RecordingType;
}
```

위와 같이 코딩했을때 얻을 수 있는 장점

1. 타입을 명시적으로 정의함으로써 다른 곳으로 값이 전달되어도 타입이 유지된다.
2. 타입을 명시적으로 정의하고 해당 타입의 의미를 설명하는 주석을 붙여 넣을 수 있다.
3. keyof연산자로 더욱 세밀하게 객체의 속성 체크가 가능해진다.

```ts
function pluck<T>(records: T[], key: keyof T): any[] {
    return records.map((r) => r[key]);
}
```

매개변수의 타입을 string을 선언할때 보다 keyof 를 활용함으로써 타입스크립트가 반환타입을 추론할 수 있게 되고, 에러도 줄일 수 있다.
