
## Spring Boot 객체 등록

Spring Boot에서는 xml 설정을 대체하는 ConfigureClass를 만들어서 AnnotationConfigApplicationContext 로  Bean 등록을 한다.
	- id = 메서드 이름

```java
AnnotationConfigApplicationContext container = 
			// 매개 변수로 지정한 패키지를 기준으로 @Configuration이 붙은 클래스들을 모두 로딩한다.
			// <context:component-scan base-package="/>
			new AnnotationConfigApplicationContext("com.fastcampus.tv");
```

```java
@Configuration
public class TVConfiguration {
```

> @Configuration을 사용하면 @Configuration, @Service, @Repository, @Controller, @Component 등이 메모리에 로드된다.

```
@Configuration
@ComponentScan(basePackages = {"com.test", "com.fastcampus"})
```

**SpringBootApplication**
```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```
위 3개가 핵심 Annotation인데, 실행하면 package를 확인하고, 같은 패키지에 대해 ComponentScan을 실행하게 된다.
