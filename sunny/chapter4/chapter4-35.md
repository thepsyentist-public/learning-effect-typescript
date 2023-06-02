# 4장 타입스크립트의 타입 시스템

## 아이템35. 데이터가 아닌, API와 명세를 보고 타입 만들기

### 읽은 내용

API나 doc문서든 명세된 내용을 기반으로 타입을 작성하는 것이 좋다.
특히 GraphQL API는 타입스크립트외 비슷한 타입 시스템을 상요하여 가능한 모든 쿼리와 인터페이스를 명시하는 스키마로 이루어진다.

-   Appllo: GraphQL쿼리를 타입스크리브 타입으로 변환해주는 도구

데이터보다나는 명세로부터 코드를 생성하는 것이 좋은 방법이다.