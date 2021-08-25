# 예외처리 (Exception)
## 특징
- Java에서 발생하는 문제는 오류(Error)와 예외(Exception)로 구분할 수 있다.

## 오류 (Error)
- 시스템 오류(Error) : 가상머신에서 발생하고, 개발자가 처리할 수 없는 오류
  - 동적 메모리가 없는 경우, 스택 오버플로우 등 
  
### 컴파일 오류 (Compile Error)
- 프로그램 코드 작성 중 발생하는 문법적인 오류
- IDE에서 컴파일 오류를 detection할 수 있다.

### 실행 오류 (Runtime Error)
- 실행 중인 프로그램이 중단되거나 의도하지 않은 동작(bug)을 하는 오류

## 예외 (Exception)
- 프로그램에서 제어할 수 있는 오류
- DB, Network Connection 등

### 중요성
- 프로그램의 비정상적인 종료를 피할 수 있다.
- log를 적절하게 남기고, 분석을 통해 원인을 파악하고 bug를 수정하여야 한다.
  - 상세하게 level별로 남겨야한다.
  
### 예외 클래스
> https://docs.oracle.com/javase/7/docs/api/java/lang/Exception.html

## 예외 처리
### try-catch문
```java
try{
    예외가 발생할 수 있는 코드
} catch (처리할 예외 타입) {
    try 블록 안에서 예외가 발생했을 때 예외처리 코드
}
```
<details>
<summary>예제코드 확인하기</summary>

```java
package ch08;

public class ArrayindexExceptionTest {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5};

        try{
            for (int i = 0 ; i <=5 ; i++){
                System.out.println(arr[i]);
            }
        } catch(ArrayIndexOutOfBoundsException e){
            System.out.println(e.getMessage());
            System.out.println(e.toString());
        }

        
    }
}

```
</details>

### try-catch-finally문
- try()블럭이 수행되면 finally() 블럭은 항상 수행된다.
  - return이 포함되어 있어도 수행된다.
  
<details>
<summary>예제 코드 확인하기</summary>

```java
package ch08;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FileExceptionHandling {
    public static void main(String[] args) {

        FileInputStream fis = null ;
        try {
            fis = new FileInputStream("a.txt");
            System.out.println("read");
        } catch (FileNotFoundException e) {
            System.out.println(e);
            return;
        } finally {
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("finally");
        }

        System.out.println("end");
    }
}
```
</details>

### try-with-resource문
- java7 이후로 ```FileInputStream```클래스가 ```AutoCloseable``` 인터페이스를 implement하여 close를 하지 않아도 자동으로 해제된다.
- java9부터 리소스는 try() 외부에서 선언하고 try(var)와 같이 사용할 수 있다.
```java
FileInputStream fis = new FileInputStream("a.text");
try(fis){
...
}
```


<details>
<summary>예제 코드 확인하기</summary>

```java
package ch08;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FileExceptionHandling {
    public static void main(String[] args) {
        // 자동으로 close가 된다.
        try (FileInputStream fis = new FileInputStream("a.txt")){
            System.out.println("read");
        // file을 불러올 때 생길 수 있는 에러
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        // file을 close할 때 생길 수 있는 에러
        } catch (IOException e) {
            e.printStackTrace();
        }
        System.out.println("end");

    }
}
```
</details>

### 예외처리 미루기
- main함수에서 throws를 하게되면 VM으로 넘어가서 abort된다.
- throws를 이용하여 예외를 발생시키는 문장에서 예외를 처리하게 미룰 수 있다.
- ```catch(Exception e)``` 구문을 통하여 default 처리를 할 수 있다.
  - catch 구문 중 가장 아랫줄에 작성해야 다른 exception을 개별 처리할 수 있다.

<details>
<summary>예제 코드 확인하기</summary>

```java
package ch08;

import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class ThrowsException {
  // throws하여 메서드를 사용할 때 Exception을 처리하도록 한다.
  public Class loadClass(String fileName, String className) throws ClassNotFoundException, FileNotFoundException {

    FileInputStream fis = new FileInputStream(fileName);
    Class c = Class.forName(className);
    return c;
  }

  public static void main(String[] args) {
    ThrowsException test = new ThrowsException();
    try {
      test.loadClass("a.txt", "abc");

    } catch (ClassNotFoundException e) {
      System.out.println(e);
    } catch (FileNotFoundException e) {
      System.out.println(e);
    } catch (Exception e) {
      System.out.println("default Exception");
    }

    System.out.println("end");
  }
}

```
</details>

### 사용자 정의 예외클래스 
- 자바에서 제공되는 exception 외 필요한 exception을 생성할 수 있다.
- 기존 클래스 중 가장 유사한 클래스를 상속을 받거나, Exception 클래스를 상속받는다.
- Exception을 정의하고, 필요한 순간에 어떠한 exception이 어느 순간에 불려야하는지 구현한다.
> throws
> - 예외를 호출하는 메서드에게 전가하는 것
> - 메서드에서 상위 메서드로 예외를 던진다.
> 
> throw
> - exception을 실제로 던지는 것
> - 메서드 내에서 상위 블럭으로 예외를 던진다.

<details>
<summary>Exception 정의하기</summary>

```java
package ch10;

public class PassWordException extends Exception{

    public PassWordException(String message){
        super(message);
    }
}
```
</details>

<details>
<summary>사용자 정의 Exception</summary>

```java
package ch10;

public class PassWordTest {

    private String password;

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) throws PassWordException {
        if(password == null) {
            throw new PassWordException("비밀번호는 null이 될 수 없습니다.");
        }
        else if (password.length() < 5 ) {
            throw new PassWordException("비밀번호는 5자 이상이어야 합니다..");
        }
        else if (password.matches("[a-zA-Z]+")){
            throw new PassWordException("비밀번호는 숫자나 특수문자를 포함해야 합니다.");
        }

        this.password = password;
    }

    public static void main(String[] args) {
        PassWordTest test = new PassWordTest();

        String password = null;

        try {
            test.setPassword(password);
            System.out.println("오류없음1");
        } catch (PassWordException e) {
            System.out.println(e.getMessage());
        }


        password = "abc";
        try {
            test.setPassword(password);
            System.out.println("오류없음2");
        } catch (PassWordException e) {
            System.out.println(e.getMessage());
        }


        password = "abcde";
        try {
            test.setPassword(password);
            System.out.println("오류없음3");
        } catch (PassWordException e) {
            System.out.println(e.getMessage());
        }

        password = "abcde1#";
        try {
            test.setPassword(password);
            System.out.println("오류없음4");
        } catch (PassWordException e) {
            System.out.println(e.getMessage());
        }

    }
}
```
</details>

