# 트랜잭션 격리 수준(Transaction Isolation Level)

[참조링크1](https://velog.io/@guswns3371/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B2%A9%EB%A6%AC%EC%88%98%EC%A4%80)

[참조링크2](https://dar0m.tistory.com/225)

---

## Isolation Level

트랜잭션에서 일관성 없는 데이터를 허용하도록 하는 수준

트랜잭션은 데이터의 정합성을 보장하기 위한 수단이다.

논리적인 수준의 작업 자체가 100% commit 되거나, 혹은 Roll back 되는 것을 보장해주어야 한다.

트랜잭션 격리수준은 ACID에서 I, Isolation(격리성)에 해당하는 파트가 된다.

동시에 실행되는 서로 다른 트랜잭션은 서로가 서로에게 영향을 미쳐서는 안된다.

동시에 같은 데이터를 수정하게 되는 경우 발생하게 될 문제들을 방지하기 위해서이다.

트랜잭션 격리 수준은 고립도와 성능의 Trade-off 를 조절한다.

개발자는 동시에 들어오는 트랜잭션을 처리하기 위해서 하나의 트랜잭션이 DB의 데이터를 다루는 동안

Lock을 걸어서 문제를 발생할 수 있는데, 물론 이런 경우에 동시성 처리에 대한 고민을 안할 수가 없다.

즉, 반드시 높은 격리수준이 좋은 것은 아니며, 개발자는 더 나은 사용자 경험을 위해 고민해야할 여지가 있다.

트랜잭션 격리수준은 크게 다음의 4 가지로 나뉜다.

---

### READ UNCOMMITTED

<br />

****_SELECT 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸리지 않는 계층_****  
****_다른 트랜잭션에서 커밋되지 않은 내용도 참조할 수 있다_****

가장 낮은 격리수준이다. 트랜잭션에서 처리 중인, 아직 커밋되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용한다.

이에 따라 Dirty Read, Non-Repeatable Read, Phantom Read 현상이 발생한다.

당연히 데이터 정합성에 문제가 많고, RDBMS 표준에서는 격리 수준으로 인정하지도 않는다.

<img src="https://velog.velcdn.com/images%2Fguswns3371%2Fpost%2F36cffd97-cbc2-4082-8c01-709a04e2567a%2Fimage.png" width=600 height=600>

---

### READ COMMITTED

<br />

****_SELECT가 실행되는 동안 해당 데이터에 Shared Lock이 걸리는 계층_****  
****_실제 테이블 값을 가져오는 것이 아니라 Undo 영역에 백업된 레코드에서 값을 가져온다_****

가장 대중적으로 선택되는 격리수준이며, 대부분의 RDB에서 기본 격리수준으로 제공된다.

트랜잭션이 수행되는 동안 다른 트랜잭션이 접근할 수 없어 대기하게 되고, COMMIT이 이루어진 트랜잭션만 조회가 가능하다.

Dirty Read가 발생하지 않지만 Non-Repeatable Read, Phantom Read 현상이 발생한다.

따라서 READ COMMITTED 격리수준에서 실행되는 SQL 문장의 결과가 무엇인지 정확히 예측하고 있어야 한다.

<img src="https://velog.velcdn.com/images%2Fguswns3371%2Fpost%2F51b88777-e797-4110-b0cd-93843551c713%2Fimage.png" width=600 height=600>

---

### REPEATABLE READ

<br />

****_트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸리는 계층_****  
****_트랜잭션이 시작되기 전에 COMMIT된 내용에 대해서만 조회할 수 있는 격리수준이다_****  
****_변경되기 전 레코드는 Undo 공간에 백업해두고 실제 레코드 값을 변경한다_****

트랜잭션이 범위 내에서 조회한 데이터 내용이 항상 동일함을 보장한다. (Dirty Read, Non-Repeatable Read 해결)

다른 사용자는 트랜잭션 영역에 해당하는 데이터에 대한 수정이 불가능하다.

자신의 트랜잭션 번호보다 낮은 트랜잭션 번호에서 COMMIT된 것만 보게 된다. 이를 통해 부정합 문제를 해결하지만

하나의 트랜잭션이 길어질 수록 Undo 영역에 저장되는 데이터가 많아져서 멀티 버전을 관리해야 한다.

그러나 영향을 미칠 정도로 트랜잭션이 오래 지속되는 경우는 거의 없어서 READ COMMITTED와 성능차이는 거의 없다.

문제는 UPDATE 부정합과 Phantom Read가 발생할 수 있다는 것이다.

```sql
START TRANSACTION; -- transaction id : 1
SELECT * FROM Member WHERE name='mingkim';

    START TRANSACTION; -- transaction id : 2
    SELECT * FROM Member WHERE name = 'mingkim';
    UPDATE Member SET name = 'Gim' WHERE name = 'mingkim';
    COMMIT;

UPDATE Member SET name = 'Kim' WHERE name = 'mingkim'; -- 0 row(s) affected
COMMIT;
```

## 2번 트랜잭션

name=Gim 으로 UPDATE한 뒤 COMMIT하였다.

REPEATABLE READ 격리수준에서는 1번 트랜잭션이 일관된 데이터를 보는 것을 보장해주기 위해서

변경되기 전 내용인 name=mingkim 을 UNDO 세그먼트에 남겨둬야 한다.

## 1번 트랜잭션

1번 트랜잭션은 UPDATE 구문을 실행하게 된다.

1번 트랜잭션이 바라보는 name=mingkim 데이터는 레코드의 데이터가 아닌 UNDO 세그먼트 영역의 데이터이고,

UNDO 세그먼트 영역의 데이터는 쓰기 잠금(write lock)을 걸 수 없다. (UPDATE 구문의 경우 변경을 수행할 ROW에 대해 LOCK을 걸어야한다.)

1번 트랜잭션은 레코드 데이터에 대해 쓰기 잠금을 시도하지만 name=mingkim인 레코드가 존재하지 않으므로

0 row(s) affected가 출력되고 아무 변경도 일어나지 않는다.


<img src="https://velog.velcdn.com/images%2Fguswns3371%2Fpost%2F76f0e189-7a69-447a-bb28-ce894e397086%2Fimage.png" width=600 height=600>

---

### Serializable

****_트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸리는 계층_****  
****_선행 트랜잭션이 특정 테이블을 읽는 경우(SELECT) 공유 잠금(shared lock) 을 걸어, 다른 트랜잭션에서 해당 테이블의 데이터를 UPDATE, DELETE, INSERT 작업을 못하도록 막는다_****

가장 엄격한 격리수준으로, 완벽한 읽기 일관성 모드를 제공하며, 다른 사용자는 트랜잭션 영역에 해당되는 데이터에 대한 수정 및 입력이 불가능하다.

동시 처리 능력이 다른 격리수준보다 떨어지고, 성능 저하가 발생하여 거의 사용되지 않는다.

---

### Dirty Read

하나의 트랜잭션(A)이 처리중인, 그러나 커밋되지 않은 데이터를 다른 트랜잭션(B)이 읽을 수 있다.
이 때 A 트랜잭션에 문제가 발생해 Rollback 되어도 B 트랜잭션은 변경된 데이터로 로직을 수행한다.

> 발생 Level: Read Uncommitted

### Non-Repeatable Read

하나의 트랜잭션에서는 Repeatable Read, 정합성을 보장해야한다. 
하나의 트랜잭션에서는 같은 데이터를 몇 번을 조회해도 동일한 결과를 받아야하는데, 
A 트랜잭션에서 동일한 SELECT 문을 여러 번 사용한다고 가정했을때, 중간에 B 트랜잭션이 데이터를 수정하게 되면
이를 보장해줄수 없게 된다.

> 발생 Level: Read Committed, Read Uncommitted

### Phantom Read

하나의 트랜잭션에서 같은 쿼리를 두 번 실행했을 경우, 첫 번째 쿼리에서 없던 유령(Phantom) 레코드가 두 번째 쿼리에서 나타나는 현상.

INSERT에 대해서만 발생하는 문제이다. 이를 방지하기 위해서는 쓰기 잠금이 필요하다. 

> 발생 Level: Repeatable Read, Read Committed, Read Uncommitted
