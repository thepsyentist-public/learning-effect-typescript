# 4장 타입스크립트의 타입 시스템

## 아이템29. 사용할 때는 너그럽게, 생성할 때는 엄격하게

### 읽은 내용

문제점이 있는 코드

```ts
interface CameraOptions {
    center?: LngLat;
    zoom?: number;
    bearing?: number;
    pitch?: number;
}

type LngLat =
    | { lng: number; lat: number }
    | { lon: number; lat: number }
    | [number, number];

type LngLatBounds =
    | { northeast: LngLat; southwest: LngLat }
    | [LngLat, LngLat]
    | [number, number, number, number];

declare function setCamera(camera: CameraOptions): void;
declare function viewportForBounds(bounds: LngLatBounds): CameraOptions;

function focusOnFeature(f: Feature) {
    const bounds = calculateBoundgbox(f);
    const camera = viewportForBounds(bounds);
    setCamera(camera);
    const {
        center: { lat, lng },
        zoom,
    } = camera; // 에러 발생 , Property 'lat', 'lng' does not exist on type 'LngLat | undefined'.
    zoom; // 타입이 number | undefined
    window.location.search = "test";
}
```

viewportForBounds의 타입 선언이 사용될 때 뿐만 아니라 만들어질 때 너무 자유로운것이 문제

> 매개변수 타입의 범위가 넓으면 사용하기 편리하지만, 반환 타입의 범위가 넓으면 불편하다.

수정 된 코드

```ts
interface LngLat {
    lng: number;
    lat: number;
}
type LngLatLike = LngLat | { lon: number; lat: number } | [number, number];

interface Camera {
    center: LngLat;
    zoom: number;
    bearing: number;
    pitch: number;
}

interface CameraOptions extends Omit<Partial<Camera>, "center"> {
    center?: LngLatLike;
}
// 다른 방법
interface CameraOptions {
    center?: LngLatLike;
    zoom?: number;
    bearing?: number;
    pitch?: number;
}

type LngLatBounds =
    | { northeast: LngLatLike; southwest: LngLatLike }
    | [LngLatLike, LngLatLike]
    | [number, number, number, number];

declare function setCamera(camera: CameraOptions): void;
declare function viewportForBounds(bounds: LngLatBounds): CameraOptions;
```

Camera가 너무 엄격하므로 조건을 완화하여 느슨한 CameraOptions타입으로 만듬
focusOnFeature함수에서도 기존 에러가 나지 않음

> 선택적 속성과 유니온 타입은 반환타입보단 매개변수 타입에 더 일반적이다.
