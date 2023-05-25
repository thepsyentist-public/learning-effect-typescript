# 3장 타입스크립트의 타입 시스템

## 아이템24. 일광성 있는 별칭 사용하기

### 읽은 내용

```ts
interface Coordinate {
  x: number;
  y: number;
}

interface BoundingBox {
  x: [number, number];
  y: [number, number];
}

interface Polygon {
  exterior: Coordinate[];
  holes: Coordinate[][];
  bbox?: BoundingBox;
}

function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
  if (polygon.bbox) {
    if (
      pt.x < polygon.bbox.x[0] ||
      pt.x > polygon.bbox.x[1] ||
      pt.y < polygon.bbox.y[0] ||
      pt.y > polygon.bbox.y[1]
    ) {
      return false;
    }
  }
  // ...
}

// 코드의 중복을 줄이기 위해 위에는 정상동작하지만, 아래와 같이 변경하면

function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
  const box = polygon.bbox;
  if (polygon.bbox) {
    if (
      pt.x < box.x[0] ||
      pt.x > box.x[1] ||
      pt.y < box.y[0] ||
      pt.y > box.y[1]
    ) {
      // 이렇게 되면 에러가 발생 객체가 undefined일수도있다.
      return false;
    }
  }
  // ...
}

function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
  const box = polygon.bbox;
  if (box) {
    // box로 바꾸는 것 만으로 에러가 해결
    if (
      pt.x < box.x[0] ||
      pt.x > box.x[1] ||
      pt.y < box.y[0] ||
      pt.y > box.y[1]
    ) {
      // 이렇게 되면 에러가 발생 객체가 undefined일수도있다.
      return false;
    }
  }
  // ...
}

// 객체 비구조화를 활용하여 간결한 문법으로 만들 수 있다.
function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
  const { bbox } = polygon;
  if (bbox) {
    const { x, y } = bbox;
    if (pt.x < x[0] || pt.x > x[1] || pt.y < y[0] || pt.y > y[1]) {
      // 이렇게 되면 에러가 발생 객체가 undefined일수도있다.
      return false;
    }
  }
  // ...
}
```

### 느낀점

- 변수에 별칭 사용시 일관되게 사용하고, 비구조화문법을 잘 활용하는 것이 중요하겠다.
