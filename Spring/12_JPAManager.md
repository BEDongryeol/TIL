## EntityManagerFactoryBuilder
- `extends Object`
- JPA EntityManagerFactory 인스턴스를 생성하기위한 builder이다.
- 생성 시 기본 설정으로 세팅되며, 1개 이상의 `LocalContainerEntityManagerFactoryBean`을 생성할 수 있>도록 해준다.

### 생성자

- EntityManagerFactoryBuilder(JpaVendorAdapter jpaVendorAdapter, Map<String,?> jpaProperties, PersistenceUnitManager persistenceUnitManager)

- EntityManagerFactoryBuilder(JpaVendorAdapter jpaVendorAdapter, Map<String,?> jpaProperties, PersistenceUnitManager persistenceUnitManager, URL persistenceUnitRootLocation)

#### Parameter
- JPAVendorAdapter : `vendor`를 연결
- Map : JPA 속성을 persistence `provider`에 전달
- URL :  `fallback`을 위한 persistence unit location
- PersistenceUnitManger: persistence unit 정보 (optional, nullable)

---

## LocalContainerEntityManagerFactoryBean
- `extends AbstractEntityManagerFactoryBean
  implements ResourceLoaderAware, LoadTimeWeaverAware`
- `JPA EntityManagerFactory`를 생성하는 `FactoryBean`
- 사용 시 Spring Context에서 EMF의 공유가 용이해진다.
  - 의존성 주입을 통해 JPA base의 DAO로 전달 가능하다.
  - persistence.xml 위치 재정의, JDBC Datasources 등 설정 가능
  - 


