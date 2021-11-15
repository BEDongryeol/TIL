> ## Spring Container의 종류

### GenericXmlApplicationContext
파일 시스템이나 클래스 경로에 있는 XML 설정 파일을 로딩하여 구동하는 컨테이너

### XmlWebApplicationContext
`웹 기반`의 스프링 애플리케이션을 개발할 때 사용하는 컨테이너


Spring Container는 Servlet Container와 유사한 역할을 한다.

`Servlet Container`는 `web.xml`파일을 로딩하고, Client에서 요청을 보내면 Servlet을 생성해서 reponse해준다.

Spring Container도 일반적으로 Container를 생성하고, `application.xml` 파일을 로딩하고, 여기서 로딩된 `POJO`로 구성된 `Bean` 객체들을 가지고 있다가 요청이 들어오면 response 해준다.

여기서 `객체 생성의 주도권이 Container로` 넘어가는 IoC가(역제어) 발생하게 된다.
- 순제어 : 모든 코드를 java 코드로 제어하는 것

