## application.properties

xml 설정을 하지 않지만, 자주 변경되는 정보들을 관리할 수 있다. **Java 코드보다 우선 순위가 높기 때문에** Java 코드에서는 Web-Application-Type을 SERVLET으로 설정하여도 아래와 같이 설정하면 NONE으로 반영된다.

`spring.main.web-application-type=none`

> ### proerties가 java 코드보다 우선 순위가 높다.

