## EntityManagerFactoryBuilder
- JPA EntityManagerFactory 인스턴스를 생성하기위한 builder이다.
- 생성 시 기본 설정으로 세팅되며, 1개 이상의 `LocalContainerEntityManagerFactoryBean`을 생성할 수 있>도록 해준다.

### 생성자

- EntityManagerFactoryBuilder(JpaVendorAdapter jpaVendorAdapter, Map<String,?> jpaProperties, PersistenceUnitManager persistenceUnitManager)

- EntityManagerFactoryBuilder(JpaVendorAdapter jpaVendorAdapter, Map<String,?> jpaProperties, PersistenceUnitManager persistenceUnitManager, URL persistenceUnitRootLocation)

#### Parameter
- JPAVendorAdapter : vendor를 연결
- Map : JPA 속성을 persistence provider에 전달
- URL :  fallback을 위한 persistence unit location
- PersistenceUnitManger: persistence unit 정보 (optional, nullable)

