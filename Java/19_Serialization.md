# 직렬화 (Serialization)

## 정의
- 인스턴스의 상태를 그대로 저장(파일), 전송(네트워크), 복원하는 방식
- 바이트 스트림의 연속으로 객체 정보를 저장하고, 복원한다.
- 보조 스트림을 활용한다.

## 인터페이스
- 직렬화는 인스턴스의 정보가 외부로 유출되는 것이다.
- 직렬화를 하려면 명시해줘야한다. (Marker interface)
    - ```implements Serializable```
    - 구현코드가 없는 interface
- 클래스 내에서 직렬화하고 싶지 않은 변수, 불가능한 변수는 ```transient```로 명시한다.
    -  default 값으로 출력된다. (String = null)

<details>
<summary>예제코드 확인하기</summary>

```java
package ch17;

import java.io.*;

class Person implements Serializable{
    String name;
    transient String job;

    public Person(){}

    public Person(String name, String job){
        this.name = name;
        this.job = job;
    }

    public String toString(){
        return name + "," + job;
    }
}

public class SerializationTest {
    public static void main(String[] args) {
        Person personLee = new Person("이순신", "대표");
        Person personKim = new Person("김유신", "상무이사");

        try (FileOutputStream fos = new FileOutputStream("serial.txt");
             ObjectOutputStream oos = new ObjectOutputStream(fos))
        {
            // Serialization
            oos.writeObject(personLee);
            oos.writeObject(personKim);

        } catch (IOException e) {
            e.printStackTrace();
        }

        try (FileInputStream fis = new FileInputStream("serial.txt");
             ObjectInputStream ois = new ObjectInputStream(fis))
        {
            Person pLee = (Person)ois.readObject();
            Person pKim = (Person)ois.readObject();
            System.out.println(pLee);
            System.out.println(pKim);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }

    }
}
```
</details>

- ```implements Externalizable```
    - 읽고 쓰는 메서드를 구현해주어야 한다.

<details>
<summary>예제 코드 확인하기</summary>

```java
package ch17;

import java.io.*;

class People implements Externalizable {
    String name;
    String job;

    public People(){}
    public People(String name, String job){
        this.name = name;
        this.job = job;
    }

    public String toString(){
        return name + ", " + job;
    }

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeUTF(name);
        out.writeUTF(job);
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        name = in.readUTF();
        job = in.readUTF();

    }
}

public class ExternalizableTest {
    public static void main(String[] args) {
        People peopleLee = new People("이순신", "대표");
        People peopleKim = new People("김유신", "상무이사");

        try (FileOutputStream fos = new FileOutputStream("external.txt");
             ObjectOutputStream oos = new ObjectOutputStream(fos))
        {
            oos.writeObject(peopleLee);
            oos.writeObject(peopleKim);
        } catch (IOException e) {
            e.printStackTrace();
        }

        try (FileInputStream fis = new FileInputStream("external.txt");
             ObjectInputStream ois = new ObjectInputStream(fis)){
            People peopleL = (People)ois.readObject();
            People peopleK = (People)ois.readObject();
            System.out.println(peopleL);
            System.out.println(peopleK);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
</details>
