# 트랜잭션의 격리 수준(isolation Level)

### 격리 수준이란?
 - 동시에 여러 트랜잭션이 처리될 때 
 - 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회되는 데터를 볼 수 있도록 허용할지 말지 결정하는 것


## 트랜잭션의 격리 수준 의 종류
 - READ UNCOMMITTED
 - READ COMMITTED
 - REPEATABLE READ
 - SERIALIZABLE

### READ UNCOMMITTED(레벨 0)

- 각 트랜잭션애서 변경 내용이 COMMIT 이나 ROLLBACK 여부에 상관없이 다른 트랜잭션에서 값을 읽을 수 있다.
- 정합성에 문제가 많은 격리 수준이기 때문에 사용하지 않는 것을 권고
  (정합성 : 서로 모순이 없이 일관되게 일치하는 것)
- COMMIT이 되지 않는 상태지만, UPDATE된 값을 다른 트랜잭션에서 읽을 수 있다.

 #### 문제점 
  - DIRTY READ : 트랜잭션 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있게 됨

### READ COMMITTED(레벨 1)

- RDB에서 대부분 기본적으로 사용되고 있는 격리 수준
- DIRTY READ 와 같은 현상은 발생하지 않음
- 실제 테이블 값을 가져오는 게 아니라, UNDO 영역에 백업된 레코드에서 값을 가져옴

#### 문제점 

- 트랜잭션 1, 2 가 있다고 가정 : 트랜잭션 1이 COMMIT 한 후 아직 끝나지 않는 트랜잭션2가 다시 테이블 값을 읽으면 값이 변경됨
- 하나의 트랜잭션에서 똑같은 SELECT 쿼리를 실행했을때는 항상 같은 값을 가져와야하는데, REAPEATABLE READ의 정합성에 어긋남
- 입금, 출금 처리가 진행되는 금전적인 처리에서 주로 발생

### REPEATABLE READ(레벨 2)

- MYSQL에서는 트랜잭션마다 트랜잭션 ID를 부여하여 트랜잭션 ID보다 작은 트랜잭션 번호에서 변경한 것만 읽게 됨
- UNDO 공간에 백업해두고 실제 레코드 값을 변경
- 백업된 데이터는 불필요하다고 판단하는 시점에 주기적 삭제
- UNDO애 백업된 레코드가 많아지면 성능 감소

- 이러한 변경방식은 MVCC(Multi Version Concurrency Control)

#### 문제점
 
- PHANTOM READ 
- 다른 트랜잭션에서 수행한 변경작업에 의해 레코드가 보였다가 안보였다가 하는 현상
- 이를 방지하기 위해서는 쓰기 잠금을 걸어야 함


### SERIALIZABLE

- 가장 단순한 격리 수준이지만 가장 엄격
- 성능 측면에서는 동시 처리성능이 가장 낮음
- SERIALIZABLE 에서는 PHANTOM READ 가 발생하지 않는다. 그러나 데이터베이스에는 거의 사용되지 않음. 


[REFERENCE](https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation)