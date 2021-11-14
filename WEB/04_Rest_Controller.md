## @Controller와 @RestController의 차이점

Spring에서 `Controller`임을 명시하기 위해 클래스 상단에 `Controller` 혹은 `RestController` 어노테이션을 붙여준다.

> #### 이 둘의 주요 차이점은 HTTP Response Body가 생성되는 방식에 있다.
>
> #### RestController는 Controller와 ResponseBody의 조합으로 생각해도 좋다.
>
> Controller : Spring MVC 모델에서 사용
> RestController : RESTful 웹 서비스에서 사


### Spring MVC 처리과정
![MVC](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbXvA4D%2FbtqW4gE9bMH%2FTzOqxMdEnRXTAVqaLre5TK%2Fimg.png)

### @Controller

```java
@Target(value=TYEP)
@Retention(value=RUNTIME)
@Documented
@Component
public @interface Controller
```

- `Spring MVC`의 Controller는 `Model` 객체를 만들어 데이터를 담아 적절한 jsp파일 등의 `View`를 반환해주는 역할을 한다.


### @RestController

```java
@Target(value=TYEP)
@Retention(value=RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController
```

- RestController는 단순히 객체를 반환하고, 객체 데이터는 JSON, XML 등의 형식으로 HTTP 응답에 담아서 전송한다.

- 뷰가 아닌 Data를 반환하여 응답에 보다 유용하게 사용된다.

- RestController 어노테이션에는 Controller와 ResponseBody 어노테이션이 달려있다.

> ### 참고
[기회는 찬스](https://dncjf64.tistory.com/288)
[Baeldung Blog](https://www.baeldung.com/spring-controller-vs-restcontroller)
