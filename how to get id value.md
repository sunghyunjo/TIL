# How to get id value with mssql

## 1. IDENT_CURRENT('특정테이블명') 
- 특정 테이블에 한정
- 특정 세션과 범위에 있는 테이블에 대한 마지막 ID 값을 반환.
- 다른 곳에서 insert가 이뤄지고 있다면, any session에 해당하는 ident_current를 반환하기에, 트랜잭션 측면에서 unsafe!!

```
SELECT IDENT_CURRENT('테이블명')  
```

## 2. @@IDENTITY
- 전체 범위에 대한 현제 세션에 있는 테이블에 대해 생성된 마지막 ID 값을 반환함
- IDENTITY 값을 SELECT한 후, 1개 이상의 테이블에 @@IDENTITY를 받아올 경우 2번째 이후부턴 동작하지 않는 문제가 발생.
(이건 좀 더 살펴볼 것)
- 연쇄적으로 발생해도 최종 id값을 반환.

```
SELECT @@IDENTITY()  
```

 
## 3. SCOPE_IDENTITY()
- 현제 세션, 범위에 있는 테이블에 대해 생성된 마지막 ID 값을 반환
- 해당 세션만 유효.

```
SELECT SCOPE_IDENTITY()  
```
