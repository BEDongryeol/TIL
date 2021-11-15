## Transaction
논리적으로 묶여있는 하나 이상의 DML(Data Manipulation Language)을 칭한다.
	- DML : Insert, Update, Delete
아래와 같이 실행하면 두 명령은 1개의 transaction으로 묶인다.
```sql
UPDATE BOARD SET TITLE='제목수정' WHERE SEQ = 1;
DELETE BOARD WHERE SEQ=3;
```
> ### Transaction으로 관리하지 않으면 하나만 성공하고, 다른 하나는 실패했을 때 오류가 발생하지만 성공한 작업은 반영된다.

`ROLLBACK`과 `COMMIT` 명령어로 transaction을 관리한다.
`COMMIT`을 실행하지않으면, 영구적으로 반영되지 않는다.
COMMIT을 하지 않은 상태에서 `ROLLBACK`을 하면 이전 상태도 돌아간다.

## Transaction 설정 방법
### 1. 네임스페이스 추가
namespace `tx`를 추가해준다.

### 2. Transaction Manager 등록
기존에 `bean`으로 등록돼있던 dataSource를 <property>를 통해 dataSource에 set해준다. class는 `"org.springframework.jdbc.datasource.DataSourceTransactionManager"`를 사용하였다.

### 3. Transaction Advice 설정
`<tx:advice>`의 transaction-manager에 2번에서 등록한 Manager를 등록하고, 속성을 설정해준다.

### 4. AOP 설정
pointcut과 연결을 해주면 마무리가 된다. 

유의할 점은 advice의 method이름을 알 수 없으므로, `<aop:config>`에서 <aop:aspecet>가 아닌 `<aop:advisor>`로 설정해준다.
