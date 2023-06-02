# 4장 타입스크립트의 타입 시스템

## 아이템28. 유효한 상태만 표현하는 타입을 지향하기

### 읽은 내용

문제점 있는 코드들 살펴보기

```ts
interface State {
    pageText: string;
    isLoading: boolean;
    error?: string;
}

function renderPage(state: State) {
    if (state.error) {
        return `ERROR!`;
    } else if (state.isLoading) {
        return `LOADING`;
    }
    return `CURRENT Page`;
}
```

-   조건이 명확히 분리되어 있지 않음
-   isLoading이 true이고 error값이 존재하면 로딩 중인지 아닌지 구분이 잘안감

```ts
async function changePage(state: State, newPage: string) {
    state.isLoading = true;
    try {
        const response = await fetch(getUrlForPage(newPage));
        if (!response.ok) {
            throw new Error("Error");
        }
        const text = await response.text();
        state.isLoading = false;
        state.pageText = text;
    } catch (e) {
        state.error = "" + e;
    }
}
```

-   문제점

1. 오류가 발생했을 때 state.isLoading을 false로 설정하는 로직이 빠져있음
2. state.error를 초기화하지 않아 페이지 전환 중에 로딩 메시지 대신 과거 오류 메시지를 보여주게 됨
3. 페이지 로딩 중 사용자가 페이지 변경시 일어나는 일에 대한 예상이 어려움, 새 페이지에 오류가 뜨거나, 응답이 오는 순서에 따라 두번째 페이지가 아닌 첫번째 페이지로 전환될 가능성이 있음

문제점을 개선한 코드

```ts
interface RequestPending {
    state: "pending";
}

interface RequestError {
    state: "pending";
    error: string;
}
interface RequestSuccess {
    state: "ok";
    pageText: string;
}

type RequestState = RequestPending | RequestError | RequestSuccess;

interface State {
    currentPage: string;
    requests: { [page: string]: RequestState };
}

// 코드는 길어졌으나, 무효한 상태를 허용하지 않도록 한다.

function renderPage(state: State) {
    const { currentPage } = state;
    const requestState = state.requests[currentPage];

    switch (requestState.state) {
        case "pending":
            return `Loading`;
        case "error":
            return "Error";
        case "ok":
            return "currentPage";
    }
}

async function changePage(state: State, newPage: string) {
    state.requests[newPage] = { state: "pending" };
    state.currentPage = newPage;
    try {
        const response = await fetch(getUrlForPage(newPage));
        if (!response.ok) {
            throw new Error("Error");
        }
        const pageText = await response.text();
        state.requests[newPage] = { state: "ok", pageText };
    } catch (e) {
        state.requests[newPage] = { state: "error", error: "" + e };
    }
}
```

> 유효한 상태와 무효한 상태를 둘 다 표현하는 타입은 혼란을 초래하기 쉽고 오류를 유발하게 된다.
> 유효한 상태만 표현하도록 하며, 코드가 길어지더라도 그렇게 하는게 좋다.
