# 4장 타입 설계

## 아이템28. 유효한 상태만 표현하는 타입을 지향하기

### 읽은 내용

타입을 잘 설계하면 코드가 직관적이다.  
타입 설계가 엉망이라면 어떠한 기억, 문서도 도움이 되지 못한다.  
효과적으로 타입을 설계하려면, 유효한 상태만 표현할 수 있는 타입을 만들어 내는 것이 가장 중요하다.  
유효한 상태와 무효한 상태를 둘 다 표현하는 타입은 혼란을 초래하기 쉽고, 오류를 유발한다.  
유효한 상태만 표현하는 타입을 지향하자. 코드가 길어지고 이해하기 어렵지만 결국 시간을 절약하고 고통을 줄여준다.

**📖 BAD CASE**  
타입 하나에 isLoading, error 상태를 같이 작성하여, 현재 어떤 상태인지 구분하기 힘들다.

```jsx
interface State {
  pageText: string;
  isLoading: boolean;
  error?: string;
}

function renderPage(state: State) {
  if (state.error) {
    return `Error! Unable to load ${currentPage}: ${state.error}`;
  } else if (state.isLoading) {
    return `Loading ${currentPage}`;
  }
  return `<h1>${currentPage}</h1> ${state.pageText}`;
}
```

**📖 GOOD CASE**

네트워크 상태를 각각 명시하여 Tagged Union을 사용했다.  
switch문으로 상태를 기준으로 분기하여 상황에 맞게 알맞게 처리했다.

```tsx
interface RequestPending {
  state: 'pending';
}

interface ReqeustError {
  state: 'error';
  error: string;
}

interface RequestSuccess {
  state: 'ok';
  pageText: string;
}

type RequestState = RequestPending | ReqeustError | RequestSuccess;

interface State {
  currentPage: string;
  requests: { [page: string]: RequestState };
}

function renderPage(state: State) {
  const { currentPage } = state;
  const requestState = state.request[currentPage];

  switch (requestState.state) {
    case 'pending':
      return `Loading ${currentPage}...`;
    case 'error':
      return `Error! Unable to load ${currentPage}: ${requestState.error}`;
    case: 'ok':
      return `<h1>${currentPage}</h1> ${requestState.pageText}`;
  }
}
```

---

## 아이템29. 사용할 때는 너그럽게, 생성할 때는 엄격하게

### 읽은 내용

> 👀 **견고성의 원칙 (Postel’s law)**  
> Be liberal in what you accept, and conservative in what you send.
> **받을 때는 관대하게, 보낼 떄는 엄격하게**  
> 받는 기능은 상상 가능한 최악의 쓰레기가 올 수도 있다고 생각하고 방어적으로 구현하자.  
> 전송 기능을 구현할 때에는 매우 엄격하게 정확한 값을 보내자.

함수의 매개변수는 타입의 범위가 넓어도 되지만, 결과 반환 시 타입의 범위가 구체적이야 한다.  
매개변수와 반환 타입의 재사용을 위해서 기본 형태(반환 타입)와 느슨한 형태(매개변수 타입)를 도입하는 것이 좋다.

```tsx
interface LngLat { lng: number; lat: number; };

type LngLatLike = LngLat
  | { lon: number; lat: number; }
  | [number, number]

interface CameraOptions {
  center?: LngLatLike;
  zoom?: number;
  bearing;: number;
  pitch?: number;
}

interface Camera {
  center: LngLat;
  zoom: number;
  bearing;: number;
  pitch: number;
}

// 느슨한 매개변수 타입
declare function setCamera(camera: CameraOptions): void;

// 엄격한 반환값 타입
declare function viewportForBounds(bounds: LngLatBounds): Camera;
```

---

## 아이템30. 문서에 타입 정보를 쓰지 않기

### 읽은 내용

타입스크립트의 타입 구문 시스템은 간결하고, 구체적이며, 쉽게 읽을 수 있도록 설계 되었다.  
주석은 동기화가 되지 않지만 **타입은 코드가 변경되면** 정보를 `동기화하도록 강제`한다.  

타입 정보를 주석으로 쓰는 것보다 타입 선언을 통해 타입을 관리하는 것이 훨씬 효율적이다.
주석과 변수명에 타입 정보를 적는 것을 피하자.  

타입이 명확하지 않는 경우는 변수명에 단위 정보를 포함하는 것을 고려해보자.

```tsx
/**
 * 전경색(foreground) 문자열을 반환합니다.
 * 0 개 또는 1 개의 매개변수를 받습니다.
 * 매개변수가 없을 때는 표준 전경색을 반환합니다.
 * 매개변수가 있을 때는 특정 페이지의 전경색을 반환합니다.
 */

// 주석과 실제 구현이 일치하지 않는다.
function getForegroundColor(page?: string) {
  return page === 'login' ? { r: 127, g: 127, b: 127 } : { r: 0, g: 0, b: 0 };
}

// 주석보다는 함수의 반환 타입을 명시하자.
function getForegroundColor(page?: string): Color {
  ...
}
```

```tsx
// 변수명에 타입을 작성하는 것은 지양하자.
const ageNum = 10;
// 👍
const age = 10;

// 👍 타입이 명확하지 않는 경우는 변수명에 단위 정보를 포함하는것을 고려하자.
const timeMs = 3000;
```

---

## 아이템31. 타입 주변에 null 값 배치하기

### 읽은 내용

> 👀 `sticktNullChecks` 옵션을 사용하면 null 값과 관련된 문제를 찾아내기 쉽고, 코드의 안전성을 높일 수 있다.

한 값의 null 여부가 다른 값의 null 여부에 암시적으로 관련되도록 설계하면 안된다.  
그리고 null 값 처리를 제대로 하지 않으면 코드에서 오류가 발생할 가능성이 크기 때문에, 이에 대한 예외처리를 꼭 해줘야 한다.  

**📖 BAD CASE**

```tsx
// 반환 값이 [undefined, undefined]로 오류가 발생할 수 있다.
function extent(nums: number[]) {
  let min, max;
  for (const num of nums) {
    if (!min) {
      min = num;
      max = num;
    } else {
      min = Math.min(min, num);
      max = Math.max(max, num);
    }
  }
  return [min, max];
}
```

**📖 GOOD CASE**

```tsx
function extent(nums: number[]) {
  let result: [number, number] | null = null;
  for (const num of nums) {
    if (!result) {
      result = [num, num];
    } else {
      result = [Math.min(num, result[0]), Math.max(num, result[1])];
    }
  }
  return result;
}

const [min, max] = extent([0, 1, 2])!; // !는 null이 아님을 단언 사용한 것이다.
```

API 작성 시에는 반환 타입을 큰 객체로 만들고 반환 타입 전체가 null 이거나 null이 아니도록 만들자.  
클래스를 만들 때는 필요한 모든 값이 준비되었을 때 생성하고, null이 존재하지 않도록 하자.

```tsx
class UserPosts {
  user: UserInfo;
  posts: Post[];

  constructor(user: UserInfo, posts: Post[]) {
    this.user = user;
    this.posts = posts;
  }

  static async init(userId: string): Promise<UserPosts> {
    const [user, posts] = await Promise.all([fetchUser(userId), fetchPostsForUser(userId)]);
    return new UserPosts(user, posts);
  }

  getUserName() {
    return this.user.name;
  }
}
```

---

## 아이템32. 유니온의 인터페이스보다는 인터페이스의 유니온 사용하기

### 읽은 내용

유니온 타입의 속성을 가지는 인터페이스를 작성 중이라면, 인터페이스의 유니온 타입을 사용하는 게 더 알맞지는 않을지 검토해야한다.

**📖 BAD CASE**  
layout의 LineLayout와 paint의 FillPaint가 같은 객체로 설정되어 있는 것은 올바르지 않다.

```tsx
interface Layer {
  layout: FillLayout | LineLayout | PointLayout;
  paint: FillPaint | LinePaint | PointPaint;
}
```

**📖 GOOD CASE**

타입의 성격에 맞게 명확하게 분리하여 사용하자.
```tsx
interface FillLayer {
  type: 'fill';
  layout: FillLayout;
  paint: FillPaint;
}

interface LineLayer {
  type: 'line';
  layout: LineLayout;
  paint: LinePaint;
}

interface PointLayer {
  type: 'paint';
  layout: PointLayout;
  paint: PointPaint;
}

type Layer = FillLayer | LineLayer | PointLayer;
```

---

## 아이템33. string 타입보다 더 구체적인 타입 사용하기

### 읽은 내용

string 타입의 범위는 매우 넓다.  
문자열을 남발하여 선언된 코드보다는 구체적인 타입을 명시하는 것이 좋다.

객체의 속성 이름을 함수 매개변수로 받을 때, string 보다는 keyof T 를 사용해서 반환 타입을 추론하자.

타입을 명시적으로 정의하여,  
전달 된 곳에 타입 정보가 유지되며 keyof 연산자로 더욱 세밀하게 객체의 속성을 체크할 수 있다.

```tsx
type RecordingType = 'live' | 'studio';

interface Album {
  artist: string;
  title: string;
  releaseDate: Date;
  recordingType: RecordingType;
}
```

제네릭과 keyof를 사용하여 함수의 타입 안전성을 높이고 명확하게 지정할 수 있다.

```tsx
// 타입이 (string|Date)[]
const pluck = <T>(records: T[], key: keyof T) => {
  return records.map((r) => r[key]);
};

// K extends keyof T를 사용하여 key 값이 T 타입의 key에 한정되도록 제한하였기 때문에
// 타입 추론이 Date[]로 명확하게 이루어진다.
const pluck = <T, K extends keyof T>(records: T[], key: K) => {
  return records.map((r) => r[key]);
};
```

---

## 아이템34. 부정확한 타입보다 미완성 타입을 사용하기

### 읽은 내용

타입 선언의 목적은 `코드 안정성`과 `가독성`을 높이기 위함이다.  
그러나 정밀하게 타입을 모델링하지 않고 부정확하게 모델링하면 오히려 코드 안전성을 떨어뜨릴 수 있다.

타입 선언시 가능한 정확하게 모델링하는것이 중요하다.  
정확하게 타입을 모델링할 수 없다면 모델링을 하지 않아야한다. any와 unknown를 구별해서 사용하자.

any타입보다는 unkonwn타입을 사용하는 것이 좋다.  
unknown타입은 모든 타입을 허용하면서 타입을 좁혀서 사용해야 하기 때문에, 타입의 안전성을 유지할 수 있다.

반면, any 타입은 모든 타입을 허용하고 타입을 좁히지 않아도 되기 때문에 코드 안전성을 보장할 수는 없다.

---

## 아이템35. 데이터가 아닌, API와 명세를 보고 타입 만들기

### 읽은 내용

코드의 구석 구석까지 타입 안전성을 얻기 위해서는 `API 또는 데이터 형식`에 대한 `타입 생성`을 고려해야한다.  
데이터에 드러나지 않는 예외적인 경우들이 문제가 될 수 있기 때문에 데이터보다 명세로부터 코드를 생성하는 것이 좋다.

---

## 아이템36. 해당 분야의 용어로 타입 이름 짓기

### 읽은 내용

> 👀 **필 칼튼(Phil karlton)**  
> 컴퓨터 과학에서 어려운 일은 단 두가지 뿐이다. `캐시 무효화`와 `이름 짓기`


타입 이름을 짓는 것은 타입 설계에서 중요하다.  
data, info, item, object, entity 같은 모호하고 의미 없는 이름을 지양하자.  
그리고 가독성을 높이고, 추상화 수준을 올리기 위해 해당 분야의 용어를 사용하자.

**📖 BAD CASE**

```tsx
interface Animal {
  name: string; // name은 매우 일반적인 용어라 동물의 학명인지 일반적인 명칭인지 알 수 없다.
  endangered: boolean; // 멸종 위기 인지 이미 멸종인지 헷갈린다.
  habitat: string; // 서식지라는 개념이 너무 범위가 넓다.
}
```

**📖 GOOD CASE**

```tsx
interface Animal {
  commonName: string;
  genus: string;
  species: string;
  status: ConservationStatus;
  climate: KoppenClimate[];
}

type ConservationStatus = 'EX' | 'EW' | 'CR' | 'EN';
type KoppenClimate = 'Af' | 'Am' | 'As' | 'Aw' | 'BSh';
```

---

## 아이템37. 공식 명칭에는 상표를 붙이기

### 읽은 내용

타입스크립트는 `구조적 타이핑(덕 타이핑)`의 특성 때문에 가끔 코드가 이상한 결과를 낼 수 있다.

```tsx
interface Vector2D {
  x: number;
  y: number;
}

function calculateNorm(p: Vector2D) {
  return Math.sqrt(p.x * p.x + p.y * p.y);
}

calculateNorm({ x: 3, y: 4 }); // 결과는 5

const vec3D = { x: 3, y: 4, z: 1 };
calculateNorm(vec3D); // 👎 결과는 5, z값이 포함되었지만 결과는 동일하다.
```

값을 구분하기 위해 공식 명칭이 필요하다면 상표를 붙이는 것을 고려하자.  
상표 기법은 타입 시스템에서 동작하지만 런타임에 상표를 검사하는 것과 동일한 효과를 얻을 수 있다.

```tsx
type Meters = number & { _brand: 'meters' };
type Secons = number & { _brand: 'secons' };

const meters = (m: number) => m as Meters;
const secons = (s: number) => s as Secons;

const oneKm = meters(1000); // 타입은 Meters
const oneMin = secons(60); // 타입은 Secons
```
