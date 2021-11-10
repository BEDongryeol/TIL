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
