## Filter

Servlet Filter는 Request가 Servlet에 도달하기 전에 요청 데이터를 원하는 형태로 전처리하거나, Response를 후처리하기 위해 사용된다.

```java
import java.io.IOException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class TimeCheckFilter implements Filter {

    public TimeCheckFilter() {
    	System.out.println("===> TimeCheckFilter 생성");
    }
  
	public void init(FilterConfig fConfig) throws ServletException {
		System.out.println("---> init() 호출");
	}
	
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		System.out.println("---> doFilter() 호출");
		chain.doFilter(request, response);   
	}

	public void destroy() {
		System.out.println("---> destroy() 호출");
	}
}

```

## Life Cycle

Filter를 생성하면 xml 파일에 아래와 같은 속성들이 추가된다
```xml
<filter>
	<display-name>timeCheck</display-name>
	<filter-name>timeCheck</filter-name>
	<filter-class>com.fastcampus.controller.common.TimeCheckFilter</filter-class>
</filter>
<filter-mapping>
	<filter-name>timeCheck</filter-name>
	<url-pattern>/getBoardList.do</url-pattern>
	<!-- <url-pattern>*.do</url-pattern>  -->
</filter-mapping>
```

### 생성
Filter는 Servlet엔진이 생성될 때 **pre-Loading**된다.
	- Servlet 엔진이 web.xml을 로딩하면서 filter의 default 생성자와, init() 메서드를 호출한다.
	- servlet 객체
		- 객체는 브라우저에서 요청이 들어올 때 생성된다.
		-  LazyLoading

### 동작
url-pattern에 요청이 들어올 때마다 doFilter가 동작한다.
	- 위 예시로 보았을 때 /getBoardList.do uri에 접근하면 호출된다.
	- `*`.do로 설정하면 해당하는 모든 uri에 요청이 들어오면 호출된다.

### 삭제
Filter의 코드를 수정하면, destory()가 호출되고 reloading된다.


