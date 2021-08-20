# Class 클래스 사용하기
- 자바의 모든 클래스와 인터페이스는 컴파일 후 class파일이 생성된다.
    - 클래스 내의 메서드, 생성자 배열 등 모든 정보가 들어있다.

- 컴파일 된 class 파일을 로드하여 객체를 동적으로 로드하고, 정보를 가져오는 메서드가 제공된다.

## 클래스 동적 로딩
### Class.forName("클래스 이름")
- ```Class c = Class.forName("java.lang.String")```
> 동적 로딩
> - 일반적으로 Class가 Local에 있는지 살펴보고 binding되어 type으로 정의하여 변수가 사용된다.
> - Compile 할 때가 아닌, 실질적으로 실행할 때 필요한 클래스를 binding하는 방법
> - ex) JDBC 드라이버에서 실행 전까지 DB의 라이브러리가 어떤 것인지 모르고, property파일 등을 읽을 때 변수로 갖고 있다가 Oracle 드라이버 이름을 String변수에 넣어준다.
- 장점 : 동적으로  원하는 클래스를 로딩할 수 있다.
- 단점 : 로딩할 때 Local에 class나 라이브러리가 없는 등 오류가 발생하면 시스템이 다운될 수 있다.

## 사용 방법
- 주로 remote에 있는 class를 호출할 때 사용한다.
    - ```Local에 클래스가 없을 때```
- 아래 코드를 ```reflection 프로그래밍```이라고 한다.
<details>
<summary>예제 코드 확인하기</summary>

```aidl
package ch04;

import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

public class StringTest {
    public static void main(String[] args) throws ClassNotFoundException {

        Class c = Class.forName("java.lang.String");
        
        // Constructor의 목록을 배열로 가져온다.
        Constructor[] cons = c.getConstructors();
        for (Constructor co : cons) {
            System.out.println(co);
        }
        // 메소드의 목록을 가져온다.
        Method[] m = c.getMethods();
        for (Method mth : m) {
            System.out.println(mth);
        }
    }
}

```
</details>


## 인스턴스 생성
- ```reflection 프로그래밍```
    - Class 클래스를 사용하여 클래스의 정보 등을 알 수 있고, 인스턴스를 생성, 메서드를 호출하는 방식의 프로그래밍
        1. 로컬 메모리에 객체가 없는 경우
        2. 원격 프로그래밍 (서로 다른 프로세스)
        3. 객체의 타입을 알 수 없는 경우
    - ```java.lang.reflect``` 패키지의 클래스를 활용하여 사용
    - 자료형을 알고 있는 경우에는 사용하지 않음

<details>
<summary> 예제 코드 확인하기 </summary>

```java
package ch04;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class ClassTest {
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {
        // 1. Class 이름으로 invoke(호출)하기
        Class c1 = Class.forName("ch04.Person");
        // Person 타입으로 Casting
        Person person = (Person)c1.newInstance();

        person.setName("Lee");
        System.out.println(person);


        // 2. 이미 생성된 객체로 invoke
        Class c2 = person.getClass();
        // 2-1. 인스턴스화, Person타입으로 Casting
        Person person2 = (Person)c2.newInstance();
        person2.setName("Kim");
        System.out.println(person2);


        // 3. 생성자를 호출하여 객체 생성
        Class[] classArray = {String.class};
        Constructor cons = c2.getConstructor(classArray);
        // 3-1. Object 배열 형태
        Object[] initargs = {"Jeong"};
        // Line23에서 불러온 생성자로 객체 생성
        Person person3 = (Person)cons.newInstance(initargs);
        System.out.println(person3);
    }
}


```
</details>



