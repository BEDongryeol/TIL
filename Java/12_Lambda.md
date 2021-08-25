# 람다식 (Lambda Expression)

## 특징
- java8부터 함수형 프로그래밍 방식을 지원하고, 이를 ```람다식```이라고 한다.
- 함수형 프로그래밍 
  - 매개 변수만을 사용하여 만드는 함수인 ```순수 함수```를 구현
  - 외부 자료를 사용하지 않으므로 side effect가 없다
  - 여러 자료를 동시에 처리하는 병렬처리가 가능하다.
    - 함수의 기능이 자료에 독립적이다.
  
## 문법
- 익명 함수 만들기
- 매개변수와 매개변수를 이용한 실행문으로 구현된다.
- java는 OOP언어이기 때문에 람다식, 함수로만 클래스를 구성할 수 없다.
  - interface의 메소드를 구현하는 방식으로 한다.
  
```java
package ch02;

@FunctionalInterface
public interface Add {

    int add(int x, int y);
}
```

```java
package ch02;

public class AddTest {

    public static void main(String[] args) {
        Add add = (x, y) -> {return x+y;};
        System.out.println(add.add(2, 3));
    }
}
```


## 람다식과 OOP 방식 비교
- 람다식에서는 FunctionalInterface 익명 내부 클래스가 생성된다.
- OOP 방식에서는 Interface를 implement하여 구현하고, 메서드를 호출한다.

```java
package ch04;

public class StringConcatTest {
    public static void main(String[] args) {
    
        String s1 = "Hello";
        String s2 = "World";
        // OOP 방식    
        StringConcatImp strImp = new StringConcatImp();
        strImp.makeString(s1, s2);
        
        // 1. 람다식 구현 방식
        StringConcat concat = (s,v)-> System.out.println(s+","+v);
        concat.makeString(s1,s2);
        // 2. 람다식 구현 방식
        StringConcat concat2 = new StringConcat() {
            @Override
            public void makeString(String s1, String s2) {
                System.out.println(s1+","+s2);
            }
        };
    }
}

```


  

  

