사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글톤이 적합하지 않다.

대신 클래스가 `여러 자원 인스턴스`를 제공해야한다.

간단하게 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨줄 수 있다.

```java
// 불변임을 보장한다.
private final Lexicon dictionary;

public SpellChecker(Lexicon dicionary){
	this.dictionary = Objects.requireNonNull(dictionary);
}
```


출처: Effective Java
