# 4장 타입스크립트의 타입 시스템

## 아이템37. 공식 명칭에는 상표를 붙이기

### 읽은 내용

상표기법 - 스타벅스가 아니라 커피 라고 표현하기

예시

```ts
type SortedList<T> = T[] & { _brand: "sorted" };

type Meters = number & { _brand: "meters" };
type Seconds = number & { _brand: "seconds" };
```

값을 세밀하기 구분하기 위하여 공식명칭이 필요하다면 상표를 고려하기
