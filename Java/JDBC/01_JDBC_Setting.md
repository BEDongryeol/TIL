> ## JDBC (Java Database Connectivity)
- Java를 이용하여 DB에 연결하고, SQL 질의 실행 및 검색 결과를 처리하는 방법과 절차에 대한 protocol이다.
- JDBC API 는 java.sql 패키지를 사용한다.

> ## JDBC 절차

H2 DateBase를 연동하였으며, DBMS에 따라 등록하는 **Driver와 Connection 인자가 달라질 수 있습니다.**

### 1. JDBC Driver Load
- DriverManager를 통해 호출할 수 있는 방법 2가지가 있다.
	1. Class path를 설정하여 객체 생성을 통해 호출한다.
	2. Class.forName("DriverName")을 통하여 외부에 있는 드라이버 클래스를 호출한다. 

### 2. Connection 생성하기
- ```import java.sql.Connection```를 호출해준다.
- Connection은 Interface이다.

```java
Connection connection = DriverManager.getConnection(DBurl, ID, PassWord);
```

### 3. Statement 객체 생성
- ```import java.sql.Statement```를 호출한다.
- Statement도 Interface이다.
```java
Statement stmt = connection.createStatement();
``` 

### 4. SQL 실행
- sql 구문을 미리 생성해놓고 실행해주면 보다 코드를 깔끔하게 정리할 수 있다.
- SELECT 질의를 실행할 때는 ```executeQuery```로 실행하고, ResultSet 객체에 저장해주어야 한다.
```java
String sql = "SELECT * FROM TABLE";
ResultSet rs = stmt.executeQuery(sql);
```

- UPDATE, INSERT, DELETE 구문을 실행할 때는 ```executeUpdate(sql)```로 실행한다. 
    - DB의 정보를 결국 update해주는 것이니까 명명되었다고 개인적으로 생각한다.
- ```executeUpdate(sql)```은 sql 구문을 통해 몇 건의 처리가 일어났는지 저장하고 있다.
```java
Statement stmt = connection.createStatement();
int count = stmt.executeUpdate(sql);
```

### 5. 객체를 닫는다.
- 실행했던 역순으로 객체를 닫아주어야 한다.
- Connection, Statement, ResultSet은 ```AutoCloseable```을 상속받고 있으므로 try-with-resource 구문을 통하여 객체를 닫는 코드를 줄일 수 있다.
```java
rs.close();
stmt.close();
conn.close();
```