> ## Listener

Listener를 생성하면 implements할 수 있는 interface 8개가 나타난다. 

**Context(Engine) 관련 2가지, Session 관련 4가지, Request 관련 인터페이스 2가지가 있다.**

### ServletContextListener

Servlet Context Listener는 ```Servlet Engine Listener```으로 볼 수 있다.

**!!! Context = Engine !!!** Servlet 엔진과 관련된 Event를 처리한다.



Listener class를 만들는 순간 web.xml에 아래 라인이 추가된다.
```
<listener>
<listener-class>com.inwoo.controller.common.ServletEngineListener
</listener-class>
</listener>
```

#### 1. web.xml 파일을 통해 서버 엔진이 생성되면 EventListener가 생성된다.
#### 2. 엔진이 생성되는 시점에 contextInitialized() 메서드가 호출된다.
#### 3. 엔진이 종료되는 시점에 contextDestroyed() 메서드가 호출된다.

> #### 엔진이 생성, 종료되는 시점에 같이 처리할 작업을 등록할 수 있다.

**DB 연동작업을 할 때 connection에 많은 시간이 소요되기 때문에, 서버 로딩과 함께 connection을 연결하면 빠르게 처리할 수 있다.**

