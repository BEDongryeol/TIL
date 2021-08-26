# Java의 유용한 클래스들
## Object Class
- 모든 클래스의 최상위 클래스
 - 모든 class는 Object클래스를 상속받는다.
 - 메서드 중 일부는 필요에 의해 재정의 할 수 있다.
- java.lang 패키지 안에 포함되어 있다.

### java.lang
- 프로그래밍 시 자동으로 import된다
- 많이 사용하는 기본 클래스들이 속한 패키지
 - String, Integer, System 등

### toString() 메서드
- 객체의 정보를 String으로 바꾸어서 사용할 때 쓰인다.
- 재정의하여 참조변수가 멤버 변수 값을 return하게 할 수 있다.
<details>
<summary>예제 코드 확인하기</summary>
```
class Book{

    private String title;
    private String author;

    public Book(String title, String author){
        this.title = title;
        this.author = author;
    }

    @Override
    public String toString() {
        return title + "," + author;
    }
}

public class BookTest {
    public static void main(String[] args){
        Book book = new Book("데미안", "헤르만 헤세");
        System.out.println(book);
    }
}

```
</details>
### equals(), hashCode() 메서드
> equals() 메서드와 hashCode() 메서드는 짝을 이룬다.
> - equals()를 overriding하면 객체가 반환하는 hash값도 overriding해줘야 한다.
> - equals에서 사용한 멤버 변수를 hashCode 값으로 반환해주면 된다.

#### equals() 메서드
- 두 인스턴스의 ```주소 값을 비교```하여 true/false를 반환
- 인스턴스가 다르더라도 논리적으로 동일한 경우 true를 반환하도록 재정의 할 수 있다.
 - 두 객체가 논리적으로 같다라고 하면 반환하는 Hash Code 값이 같아야한다.
 - Java에서 주소 값은 ```Hash Code(해쉬 값)```이라고 한다.
  - heap 메모리를 관리하는 방식이 Hash방식이다.

#### hashCode() 메서드
- 인스턴스의 저장 주소를 반환한다.
- hash : 정보를 저장, 검색하는 자료 구조
- 자료의 특정 값(key)에 대한 저장 위치를 반환해주는 hash함수를 사용한다.
- ```index(저장위치) = hash(key)```

<details>
<summary> Overriding 코드 확인하기 </summary>
 
```
package ch02;

public class Student {
    private int studentNum;
    private String studentName;

    public Student(int StudentNum, String studentName){
        this.studentNum = studentNum;
        this.studentName = studentName;
    }

    @Override
    public String toString(){
        return studentNum + "," + studentName;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Student) {
            // 다운캐스팅
            Student std = (Student)obj;
            if (this.studentNum == std.studentNum){
                return true;
            }
        }

        return false;
    }

    @Override
    public int hashCode() {
        return studentNum;
    }
}



```

<summary>예제 코드 확인하기</summary>
 
```
package ch02;

public class EqualsTest {
    public static void main(String[] args){

        Student std1 = new Student(200, "Lee");
        Student std2 = new Student(200, "Lee");

        System.out.println(std1==std2); //false
        System.out.println(std1.equals(std2)); //true
        // hashCode() 가 studentNum을 return하도록 overriding하였음.
        System.out.println(std1.hashCode());
        System.out.println(std2.hashCode());

        // 원래 hashCode값 출력하는 방법
        System.out.println(System.identityHashCode(std1));
        System.out.println(System.identityHashCode(std2));

    }
}
```
<details>


### clone() 메서드
- 객체를 생성자를 통해 생성할 때, clone()을 사용하면 원본 객체와 원본을 복제하는데 사용한다.
 - 생성자 : 초기값을 가지고 생성이 된다.
 - clone() : 중간에 멤버변수가 변하면 변한 값을 그대로 복제한다.
- private까지 모두 복제가 되어 객체 보호의 관점에서 위배할 수 있다.
 - 명시적으로 clone() 메서드의 사용을 허용한다는 의미로 ```Cloneable``` interface를 명시해준다.
  - ```public class A implements Cloneable {~}```
 - A 클래스 내에서 clone() 메서드를 Override한다.

```
@Override
protected Object clone() throws CloneNotSupportedException {
    return super.clone();
}
```
 
 - 클론 코드 : ```Student copyStd = (Student)std1.clone();```

 
