# 트랜잭션의 특성(ACID)를 지원하는 DBMS 기능

                DBMS 기능
### 원자성   -   회복 기능
### 일관성   -   병행 제어 기능
### 격리성   -   병행제어 기능
### 지속성   -   회복기능


## 트랜잭션의 상태

*활동 상태(active)*
- 트랜잭션이 수행되기 시작하여 현재 수행중인 상태

*부분 완료 상태 (partially committed)*
- 트랜잭션의 마지막 연산이 실행을 끝낸 직후의 상태

*완료 상태(committed)*
- 트랜잭션이 성공적으로 완료되어 commit 연산을 실행한 상태
- 트랜잭션이 수행한 최종 결과를 데이터베이스에 반영하고, 데이터베이스가새로운 일관된 상태가 되면서 트랜잭션이 종료됨

*실패 상태(failed)*
- 장애가 발생하여 트랜잭션의 수행이 중단된 상태

*철회 상태(aborted)*
- 트랜잭션의 수행 실패로 rollback 연산을 실행한 상태
- 지금 까지 실행한 트랜잭션의 연산을 모두 취소하고 트랜잭션이 수행되기 전의 데이터베이스 상태로 되돌리면서 트랜잭션이 종료됨

<img src="https://user-images.githubusercontent.com/45676906/98506407-be6e2a00-229e-11eb-82a0-829aecd423dc.png">

## 트랜잭션의 종류

#### 설정 모드에 따라 3가지로 구분


### 1. 명시적 트랜잭션
- 트랜잭션 시작과 끝은 사용자가 명시적으로 구분
- **사용자 트랜잭션** 또는 **수동 트랜잭션**


```
START TRANSACTION
    . . .
    SQL 명령문;
    . . .
COMMIT || ROLLBACK;
```

```
START TRANSACTION;
       INSERT INTO 학생(학번, 이름, 주소, 학년, 나이, 성별) VALUES ("s004", "규니", "규니집", 3, 25, "남");
  SELECT * FROM 학생;
ROLLBACK;
SELECT * FROM 학생
```
위 코드에서 rollback 이전 INSERT 명령은 실행되지 않는다.


### 2. 자동완료 트랜잭션
- SQL문 하나를 독립된 하나의 트랜잭션으로 자동 정의하고 SQL문의 실행 결과에 따라 자동으로 커밋 또는 롤백하는 트랜잭션
- DBMS가 각 SQL 문장 앞뒤에 START TRANSACTION 문과 COMMIT 문 또는 ROLLBACK문을 자동으로 붙여 실행한다.
- MYSQL에서 명령을 실행하면 다른 처리 없이도 그대로 반영된다. 모든 명령이 자동으로 COMMIT 되는 것

```
DELTE FROM 학생 WHERE 학변 = '201901730';
SELECT * FROM 학생;
```

MYSQL의 디폴트 값이 Auto commit 값이 1이기 때문에 바로 쿼리문이 반영


### 3. 수동완료 트랜잭션
- 트랜잭션의 끝만 사용자가 직접 명시적으로 지정하는 트랜잭션
- 암시적 트랜잭션

```
SET AUTOCOMMIT = 0;
SELECT @@AUTOCOMMIT; 
```
오토 커밋 값을 0으로 바꿔준다면 쿼리를 실행해도 COMMIT 이 자동으로 되지 않아 데이터베이스에 바로 반영이 되지 않는다. 따라서 ROLLBACK명령어도 사용할 수 있다.



```
DELETE FROM 학생 WHERE 학번 = '201901730';
SELECT * FROM 학생;
         INSERT INTO 학생(학번, 이름, 주소, 학년, 나이, 성별) VALUES ("s004", "규니", "규니집", 3, 25, "남");
ROLLBACK;
SELECT * FROM 학생; 
```
커밋이 자동으로 이루어지지 않기 때문에 Rollback 연산이 가능하다. 이전상태로 돌아감. 