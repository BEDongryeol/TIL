## AutoConfigure

`@SpringBootApplication`은 아래와 같이 구성되어 있다.

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

여기서 `@EnableAutoConfiguration`이 /META-INF/spring.factories 파일에 등록된 호출을 해준다.

덕분에 개발자는 많은 xml 설정을 하지 않고, 개발에만 집중을 할 수 있다.

### 종류

AutoConfigure의 종류는 `Unconditional`, `Positive`, `Negative`로 나뉜다.

예시로 `WebMvcAutoConfiguration`를 타고 들어가보면 `@ConditionalOnWebApplication`, 
`@ConditionalOnMissingBean`, `@ConditionalOnClass` 등의 어노테이션을 통해 **조건부로 호출**이 되게 구성이 되어 있다.

먼저 처리되어야 할 `AutoConfigure`을 명시하는 등 순서와 조건 또한 포함하고 있다.


Main 클래스의 `@SpringBootApplication` 중 `@ComponentScan`이 `@EnableAutoConfiguration` 보다 먼저 실행된다.

JDBC관련 초기화 등을 Override해도 덮어 씌워지지 않는다.

> 해결 방법 : custom starter의 auto configuration 클래스에서 
`@ConditionalOnMissingBean`를 선언한다.
>
> @ConditionalOnMissingBean : 메모리에 해당 타입의 객체가 없다면 bean으로 등록
