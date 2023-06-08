# 5장 any 다루기

## 아이템44. 타입 커버리지를 추적하여 타입 안정성 유지하기

### 읽은 내용

npm의 type-cover-age 패키지를 활용하여 any를 추적할 수 있는 방법 존재

```
npx type-coverage  // 백분율이 나오는데, 백분율이 100%에 가까울수록 any가 없다는것
```

```
npx type-coverage --detail  // any타입이 있는 곳을 모두 출력
```
