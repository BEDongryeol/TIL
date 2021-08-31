# Log (로그)
## Logging
- 시스템 운영에 대한 기록
  - 오류가 발생했을 때 오류에 대한 기록을 남겨 디버깅을 용이하게 할 수 있다.
- ```java.util.logging.Logger``` 활용
- Exception을 handling할 때 로그를 남겨주어야 한다.
  - Log가 너무 많으면 빈번한 file 입출력의 오버헤드 및 백업 문제 등이 발생할 수 있다.

## 레벨
- severe
- warning
- info
- config
- fine
- finer
- finest

## 사용
- 처음 System을 오픈할 때는 모든 level을 찍고, 안정화된 후에 info level부터 찍는 등 다양한 방법이 있다.
- logger의 level을 ALL로 설정하면 다른 FileHandler는 모든 범위 내에서 설정이 가능하다.
- 같은 패키지 내에 로그 파일이 저장된다.

<details>
<summary>예제코드1 확인하기</summary>

```java
package Online;

import java.io.IOException;
import java.util.logging.FileHandler;
import java.util.logging.Level;
import java.util.logging.Logger;
import java.util.logging.SimpleFormatter;

public class MyLog {
    // 1. Singleton과 유사하게 인스턴스 생성
    Logger logger = Logger.getLogger("mylogger");
    private static MyLog instance = new MyLog();


    // 2. 파일 이름 설정
    public static final String errorLog = "log.txt";
    public static final String warningLog = "warning.txt";
    public static final String fineLog = "fine.txt";

    // 3. 위 파일별 log를 설정할 FileHandler 객체 생성
    // 객체명은 오류 레벨과 맞춰서 설정하였음
    private FileHandler logFile = null;
    private FileHandler warningFile = null;
    private FileHandler fineFile = null;

    // 4. 생성자 구현
    private MyLog(){
        try {
            // 4-1. 인스턴스 생성 (파일이름, append 여부)
            logFile = new FileHandler(errorLog, true);
            warningFile = new FileHandler(warningLog, true);
            fineFile = new FileHandler(fineLog, true);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (SecurityException e) {
            e.printStackTrace();
        }
        // 4-2. Log기록의 형식 지정 (SimpleFormatter : 기본 형식)
        logFile.setFormatter(new SimpleFormatter());
        warningFile.setFormatter(new SimpleFormatter());
        fineFile.setFormatter(new SimpleFormatter());

        // 4-3. 로그 레벨 설정
        // 모든 로그 레벨 기록
        logger.setLevel(Level.ALL);
        // FileHandler마다 레벨을 다르게 설정할 수 있다.
        warningFile.setLevel(Level.WARNING);
        fineFile.setLevel(Level.FINE);;

        // 4-4. 4-1에서 생성한 Handler 추가
        logger.addHandler(logFile);
        logger.addHandler(warningFile);
        logger.addHandler(fineFile);
    }

    // 4-5. getter 생성
    public static MyLog getLog() {
        return instance;
    }

    // 5. 로그 기록 남기는 함수
    // logger에 설정한 레벨에 해당하는 log만 사용가능
    public void log(String msg){
        // logger가 finest level을 handle 할 수 있도록 addHandler를 해주었으므로 가능하다.
        logger.finest(msg);
        logger.finer(msg);
        logger.fine(msg);
        logger.config(msg);
        logger.info(msg);
        logger.warning(msg);
        logger.severe(msg);
    }
    // 5-1. fine level 메세지 출력함수
    public void fine(String msg){
        logger.fine(msg);
    }

    // 5-2. warning level 메시지 출력함수
    public void warning(String msg){
        logger.warning(msg);
    }
}
```
</details>


- logger의 level을 INFO로 설정하면 다른 FileHandler는 INFO 내에서 설정이 가능하다.
  - FileHandler 중 fineFile은 범위를 fine부터 설정하였지만 INFO 범위 내에 포함되지 않으므로 INFO 레벨부터 로그가 기록된다.

<details>
<summary>예제코드2 확인하기</summary>

```java
package Online;

import java.io.IOException;
import java.util.logging.FileHandler;
import java.util.logging.Level;
import java.util.logging.Logger;
import java.util.logging.SimpleFormatter;

public class MyLog {
    // 1. Singleton과 유사하게 인스턴스 생성
    Logger logger = Logger.getLogger("mylogger");
    private static MyLog instance = new MyLog();


    // 2. 파일 이름 설정
    public static final String errorLog = "log.txt";
    public static final String warningLog = "warning.txt";
    public static final String fineLog = "fine.txt";

    // 3. 위 파일별 log를 설정할 FileHandler 객체 생성
    // 객체명은 오류 레벨과 맞춰서 설정하였음
    private FileHandler logFile = null;
    private FileHandler warningFile = null;
    private FileHandler fineFile = null;

    // 4. 생성자 구현
    private MyLog(){
        try {
            // 4-1. 인스턴스 생성 (파일이름, append 여부)
            logFile = new FileHandler(errorLog, true);
            warningFile = new FileHandler(warningLog, true);
            fineFile = new FileHandler(fineLog, true);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (SecurityException e) {
            e.printStackTrace();
        }
        // 4-2. Log기록의 형식 지정 (SimpleFormatter : 기본 형식)
        logFile.setFormatter(new SimpleFormatter());
        warningFile.setFormatter(new SimpleFormatter());
        fineFile.setFormatter(new SimpleFormatter());

        // 4-3. 로그 레벨 설정
        // 모든 로그 레벨 기록
        logger.setLevel(Level.INFO);
        // FileHandler마다 레벨을 다르게 설정할 수 있다.
        warningFile.setLevel(Level.WARNING);
        fineFile.setLevel(Level.FINE);;

        // 4-4. 4-1에서 생성한 Handler 추가
        logger.addHandler(logFile);
        logger.addHandler(warningFile);
        logger.addHandler(fineFile);
    }

    // 4-5. getter 생성
    public static MyLog getLog() {
        return instance;
    }

    // 5. 로그 기록 남기는 함수
    // logger에 설정한 레벨에 해당하는 log만 사용가능
    public void log(String msg){
        // logger가 finest level을 handle 할 수 있도록 addHandler를 해주었으므로 가능하다.
        logger.finest(msg);
        logger.finer(msg);
        logger.fine(msg);
        logger.config(msg);
        logger.info(msg);
        logger.warning(msg);
        logger.severe(msg);
    }
    // 5-1. fine level 메세지 출력함수
    public void fine(String msg){
        logger.fine(msg);
    }

    // 5-2. warning level 메시지 출력함수
    public void warning(String msg){
        logger.warning(msg);
    }
}
```
</details>

<details>
<summary>테스트 코드</summary>

```java
package Online;

public class LogTest {
    public static void main(String[] args) {

        // Singleton으로 구현하였으므로 getLog를 통해서 호출
        MyLog logger = MyLog.getLog();
        logger.log("Log Test");
    }
}
```
</details>

- 콘솔 파일은 Mac OS 기준으로 ```jre 내의 conf 폴더```에 properties파일이 저장된다.
- 파일 내에서 정보 date, Thread Name 등을 설정할 수 있다.

<details>
<summary>콘솔 파일 내용 확인하기</summary>

```properties
############################################################
#  	Default Logging Configuration File
#
# You can use a different file by specifying a filename
# with the java.util.logging.config.file system property.  
# For example java -Djava.util.logging.config.file=myfile
############################################################

############################################################
#  	Global properties
############################################################

# "handlers" specifies a comma separated list of log Handler 
# classes.  These handlers will be installed during VM startup.
# Note that these classes must be on the system classpath.
# By default we only configure a ConsoleHandler, which will only
# show messages at the INFO and above levels.
handlers= java.util.logging.ConsoleHandler

# To also add the FileHandler, use the following line instead.
#handlers= java.util.logging.FileHandler, java.util.logging.ConsoleHandler

# Default global logging level.
# This specifies which kinds of events are logged across
# all loggers.  For any given facility this global level
# can be overriden by a facility specific level
# Note that the ConsoleHandler also has a separate level
# setting to limit messages printed to the console.
.level= INFO

############################################################
# Handler specific properties.
# Describes specific configuration info for Handlers.
############################################################

# default file output is in user's home directory.
java.util.logging.FileHandler.pattern = %h/java%u.log
java.util.logging.FileHandler.limit = 50000
java.util.logging.FileHandler.count = 1
# Default number of locks FileHandler can obtain synchronously.
# This specifies maximum number of attempts to obtain lock file by FileHandler
# implemented by incrementing the unique field %u as per FileHandler API documentation.
java.util.logging.FileHandler.maxLocks = 100
java.util.logging.FileHandler.formatter = java.util.logging.XMLFormatter

# Limit the message that are printed on the console to INFO and above.
java.util.logging.ConsoleHandler.level = INFO
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter

# Example to customize the SimpleFormatter output format 
# to print one-line log message like this:
#     <level>: <log message> [<date/time>]
#
# java.util.logging.SimpleFormatter.format=%4$s: %5$s [%1$tc]%n

############################################################
# Facility specific properties.
# Provides extra control for each logger.
############################################################

# For example, set the com.xyz.foo logger to only log SEVERE
# messages:
# com.xyz.foo.level = SEVERE

```
</details>

## Exception과 함께 사용
- Exception 상황에서는 항상 적절한 level의 log를 남겨야한다.

<details>
<summary>Custom Exception 코드</summary>

```java
package Online;

public class StudentNameFormatException extends IllegalArgumentException{
    public StudentNameFormatException(String message){
        super(message);
    }
}

```
</details>

<details>
<summary>예제 클래스 </summary>

```java
package Online;

public class Student {
  private String studentName;
  MyLog myLog = MyLog.getLog();

  public Student(String studentName){

    if (studentName == null){
      throw new StudentNameFormatException("Name must not be null");
    }
    if (studentName.split(" ").length > 3) {
      throw new StudentNameFormatException("너무 긴 이름입니다.");
    }

    this.studentName = studentName;
  }

  public String getStudentName(){
    myLog.fine("begin getStudentName");
    return studentName;
  }
}
```
</details>

- 생성자에는 Exception을 throws하지 않는다.
- Custom Exception의 extends에 Exception이 아닌 ```IllegalArgumentException```을 extends하여 핸들링할 수 있다.

<details>
<summary>테스트 코드 확인하기</summary>

```java
package Online;

public class StudentTest {
    public static void main(String[] args) {

        MyLog myLog = MyLog.getLog();

        String name = null;
        // 1. 예외상황1 테스트
        try {
            Student student = new Student(name);
        } catch (StudentNameFormatException e) {
            myLog.warning(e.getMessage());
        }
        // 2. 예외상황2 테스트
        try {
            Student student = new Student("Edward John Kim Test");
        } catch (StudentNameFormatException e) {
            myLog.warning(e.getMessage());
        }

        Student student = new Student("James");
    }
}
```
</details>

<details>
<summary>결과 확인하기</summary>

```java
8월 31, 2021 3:24:58 오후 Online.MyLog warning
WARNING: Name must not be null
8월 31, 2021 3:24:58 오후 Online.MyLog warning
WARNING: 너무 긴 이름입니다.
```
</details>

