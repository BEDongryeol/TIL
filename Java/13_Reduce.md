# reduce 메서드
## reduce() 연산
- 기존에 정의된 연산이 아닌 직접 구현한 연산을 적용한다.
```java
T reduce(T identify, BinaryOperator<T> accumulator)
```
- 최종 연산으로 스트림을 요소를 소모하여 연산을 수행한다.
- 예시
```java
package ch06;

import java.util.Arrays;
import java.util.function.BinaryOperator;
// parameter BinaryOperator<T>를 구현하여 사용
class CompareString implements BinaryOperator<String>{

    @Override
    public String apply(String s1, String s2) {
        if (s1.getBytes().length >= s2.getBytes().length) return s1;
        else return s2;
    }
}


public class ReduceTest {
    public static void main(String[] args) {

        String greetings[] = {"안녕히계세요~~~", "hello", "Goood morning", "반갑습니다"};

        // 1. 직접 작성
        System.out.println(Arrays.stream(greetings).reduce("", (s1,s2)->
            {if (s1.getBytes().length >= s2.getBytes().length) return s1;
            else return s2;}
        ));

        // 2. BinaryOperator<T>의 메서드 implement
        // 새로운 클래스로 생성하여 사용한다.
        String str = Arrays.stream(greetings).reduce(new CompareString()).get();
        System.out.println(str);



    }
}

```
