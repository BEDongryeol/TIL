## JPA Caution

#### hibernate는 아래 파일을 읽어들이도록 설정이 되어 있다.
- src/main/resources/META-INF/persistence.xml

`DML`(INSERT, UPDATE, DELETE) 작업은 반드시 `트랜잭션 안`에서 실행되어야한다.

```java
EntityTransaction tx = em.getTransaction();
tx.begin();
em.persist(board);
tx.commit();
```
