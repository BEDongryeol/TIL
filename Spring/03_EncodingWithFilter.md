## Filter

Spring Framework에서 Filter와 Listener는 `preloading`된다.

따라서 DB 연동, AOP 등을 미리 호출해놓고, 요청이 들어올 때 빠르게 처리할 수 있다.

`Filter`로 설정하기 위해서는 `@WebFilter` 어노테이션이 필요하다.

속성으로 `urlPatterns`, `initParams`를 설정할 수 있다. InitParams 내부에 `@WebInitParam` 에 name과 value를 추가적으로 설정할 수 있다.

## Filter를 통한 Encoding

### 1. Annotation을 통해 WebInitParam들을 설정해준다.

### 2. 값을 받을 변수를 내부적으로 생성하고, `init()`을 통해 호출될 때 key를 호출하여 value를 저장해준다.

### 3. `chain.doFilter()` 실행 전에 request를 받아 ChracterEncoding을 set해준다.

```java
@WebFilter(urlPatterns = "*.do",
		   initParams = {@WebInitParam(name = "encoding", value = "EUC-KR"),
				   		 @WebInitParam(name = "message", value = "Hello")})
public class CharacterEncodingFilter implements Filter {
	private String boardEncoding;
	
    public CharacterEncodingFilter() {
    	System.out.println("===> CharacterEncodingFilter 생성");
    }
    public void init(FilterConfig fConfig) throws ServletException {
		boardEncoding = fConfig.getInitParameter("encoding");
	}
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// 모든 서블릿이 실행되기 전에 인코딩을 일괄적으로 처리한다.
		request.setCharacterEncoding(boardEncoding);
		// chain.doFilter()가 호출되는 순간 그 다음 필터가 실행되고, 없으면 Servlet이 실행된다.	
		chain.doFilter(request, response);		
	}
	
	public void destroy() {}
}
```
