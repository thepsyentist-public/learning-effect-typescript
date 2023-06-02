# 4장 타입스크립트의 타입 시스템

## 아이템31. 타입 주변에 null 값 배치하기

### 읽은 내용

```ts
function extent(nums: number[]) {
    let min, max;
    for (const num of nums) {
        if (!min) {
            min = num;
            max = num;
        } else {
            min = Math.min(min, num);
            max = Math.max(max, num); // 오류 발생 , Argument of type 'number | undefined' is not assignable to parameter of type 'number'. Type 'undefined' is not assignable to type 'number'.
        }
    }
    return [min, max];
}
```

위 코드의 문제점.
min의 경우만 제외하고 max에서는 제외를 하지 않아 에러 발생

```ts
function extent(nums: number[]) {
    let result: [number, number] | null = null;
    for (const num of nums) {
        if (!result) {
            result = [num, num];
        } else {
            result[(Math.min(num, result[0]), Math.max(num, result[1]))];
        }
    }
    return result;
}
```

반환 타입을 [number, number] | null 로 하여 오류가 발생하지 않도록함

또 하나의 예제

```ts
class UserPosts {
    user: UserInfo | null;
    posts: Post[] | null;

    constructor() {
        this.user = null;
        this.posts = null;
    }

    async init(userId: string) {
        return Promise.all([
            async () => (this.user = await fetchUser(userId)),
            async () => (this.posts = await fetchPost(userId)),
        ]);
    }

    getUserName() {
        // ...
    }
}
```

user와 posts속성은 api요청시 로드되는동안 null 상태이며, 어떤시점에는 둘다 null , 하나만 null, 둘다 null이 아닌 상황이 발생

개선된 코드

```ts
class UserPosts {
    user: UserInfo;
    posts: Post[];

    constructor(user: UserInfo, posts: Post[]) {
        this.user = user;
        this.posts = posts;
    }

    static async init(userId: string): Promise<UserPosts> {
        const [user, posts] = await Promise.all([
            fetchUser(userId),
            fetchPost(userId),
        ]);
        return new UserPosts(user, posts);
    }

    getUserName() {
        return this.user.name;
    }
}
```

이 클래스는 null이 아니게 되었고, 메서드를 작성하기 쉬워진 상태가 되었음.
\*null인 경우가 필요한 속성은 프로미스로 바꾸면 안된다.

API 작성 시에는 반환 타입을 큰 객체로 만들고 반환 타입 전체가 null이거나 null이 아니게 만들어야한다.
클래스를 만들때는 null이 존재하지 않도록 하는 것이 좋다.
