## Mybatis

Java 코드 1줄로 DB 연동을 실행할 수 있다.

spring jdbc는 select 결과를 어떤 객체를 생성해서 어떻게 넣어줄 지 기술

Mybatis는 Mapping도 자동으로 해주는데, sql query 들이 담겨잇는 xml 파일이 있어야한다.

SELECT에는 resultType 등 RESULT 관련 속성을 추가해서 mapping을 해주어야한다.

---

### Configuration

#### 1. container가 configuration의 mapper의 resource 속성에 있는 `xml 파일`들을 로드한다.

#### 2. 여러 xml파일에도 `id는 겹치면 안된다.` 전부 다 달라야한다.
- namespace를 설정해주면 오류가 안난다. A.a B.a 구분이 가능하기 때문에
- parameter type은 생략이 가능하다.
- xml에서 작다 크다 >, <를 사용할 수 없고, >를 &gt;로 바꿔주거나 CDATA Section을 통해 설정할 수 있다.

```XML
<![CDATA[
		DELETE BOARD 
		WHERE SEQ <= #{seq}
	]]>
```


#### 3. 의존성 주입 시에 `sql session template`(Mybatis Container)이 필요하다.

#### etc.
2001년 iBatis 2005년 Spring 2010년 MyBatis

Spring-iBatis를 연결할 때는 Springframework에서 제공하는 클래스 사용.

Spring-Mybatis를 연결할 때는 Mybatis에서 제공하는 클래스를 사용.

---

### Factory Bean

Factory Bean은 xml파일들이 들어있는 mapper도 연동해주고, datasource도 연결해준다.

보통 property를 이용해서 setter injection을 쓰는데,

mybatis spring sqlsessiontemplate에는 setterMethod가 없어서, contruct-args로 주입을 해준다.

---


