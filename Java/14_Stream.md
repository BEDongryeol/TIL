# 스트림 (Stream)
## 특징
- 배열, collection 자료가 모여있을 때 연산의 처리를 일관성 있게 한다.
  - 자료 처리에 대한 추상화가 구현되었다고 한다.
- 일관성 있는 연산으로 자료의 처리를 쉽고 간단하게 한다.
- 한번 생성하고 사용한 스트림은 재사용 할 수 없다.
  - 다른 연산을 수행하기 위해서는 스트림을 다시 생성해야 한다.

```java
package ch06;

import java.util.Arrays;
import java.util.stream.IntStream;

public class IntArrayStreamTest {
    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 5};
        for (int num : arr) {
            System.out.println(num);
        }
        System.out.println("========");
        // 모든 Array는 Arrays 클래스를 활용할 수 있다.
        IntStream is = Arrays.stream(arr);
        is.forEach(n -> System.out.println(n));
        //is는 한번 소모하였으니 다시 사용할 수 없다.
        // 재사용하고 싶을 때 재생성
        int sum = Arrays.stream(arr).sum();
        System.out.println(sum);

    }
}

```

## 연산
- 스트림의 연산은 중간 연산과 최종 연산으로 구분된다.
- 중간 연산은 여러 개의 연산이 적용될 수 있지만 최종 연산은 마지마게 한 번만 적용된다.
- ```지연 연산```
  - 최종 연산이 호출되어야 중간 연산에 대한 수행이 이루어지고 결과가 만들어진다. 

### 중간연산
- ```filter()``` : 조건에 맞는 요소를 추출
- ```map()``` : 조건에 맞는 요소를 변환
- ```sorted()``` : 정렬

### 최종연산
- ```forEach()``` : 요소를 하나씩 꺼내옴 
- ```count()``` : 요소의 개수 반환
- ```sum()``` : 요소들의 합

<details>
<summary>예제 코드 확인하기</summary>

```java
package ch06;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

public class ArrayListStreamTest {
    public static void main(String[] args) {

        List<String> sList = new ArrayList<String>();
        sList.add("Thomas");
        sList.add("Edward");
        sList.add("Jack");

        Stream<String> stream = sList.stream();
        stream.forEach(s->System.out.println(s));

        sList.stream().sorted().forEach(s->System.out.print(s + "\t"));
        System.out.println();
        sList.stream().map(s->s.length()).forEach(n->System.out.print(n + "\t"));
        System.out.println();
        sList.stream().filter(s->s.length()>=5).forEach(s->System.out.print(s + "\t"));
    }
}

// 결과
//Thomas
//Edward
//Jack
//Edward	Jack	Thomas
//6 6	4
//Thomas	Edward	
```
</details>


