# IO 스트림
## IO Stream (입출력 스트림)
- 스트림 : 네트워크에서 자료의 흐름이 물의 흐름과 같다는 비유에서 유래
- java의 입출력을 위한 스트림
- 입출력 매개체에 독립적으로 일관성있는 입출력을 제공한다.
  - 키보드, 마우스, 네트워크, 메모리 등

## 구분
### 대상 기준
- 입력과 출력 스트림은 독립적으로 구분된다.

<details>
<Summary>대상 기준에 따른 스트림</Summary>

#### 입력 스트림(입력용)
- FileInputStream
- FileReader
- BufferedInputStream
- BufferedReader ...

#### 출력 스트림(출력용)
- FileOutputStream
- FileWriter
- BufferedOutputStream
- BufferedWriter ...
</details>


### 자료의 종류
<details>
<summary>자료의 종류에 따른 스트림</summary>

#### 바이트 스트림
- 동영상, 음악 파일, 실행 파일 등을 읽고 쓸 때 사용
- FileInputStream
- FileOutputStream
- BufferedInputStream
- BufferedOutputStream ...

#### 문자 스트림
- 바이트 단위로 자료를 처리하면 문자는 깨진다.
- 인코딩에 맞게 2바이트 이상으로 처리할 때 사용
- FileReader
- FileWriter
- BufferedReader
- BufferedWriter ...
</details>

### 기능
<details>
<summary>기능에 따른 스트림</summary>

#### 기반 스트림
- 대상에 직접 자료를 읽고 쓰는 기능의 스트림
- FileInputStream
- FileOutputStream
- FileReader
- FileWriter

#### 보조 스트림
- 실제로 읽고 쓰는 기능은 없으나, 다른 스트림을 감싸서(wrap), 다른 스트림이 하는 일을 보조해준다.
- 다른 기반 스트림이나, 보조 스트림을 생성자의 매개변수로 갖는다.
- InputStreamReader
- OutputStreamWriter
- BufferedInputStream
- BufferedOutputStream
</details>

## 표준 입출력 스트림
- System.out
  - 표준 출력(모니터) 스트림
  - ```System.out.println("출력 메세지");```

- System.in
  - 표준 입력(키보드) 스트림
  - ```int d = System.in.read() // 한 바이트 읽기```
  > 한 바이트로 읽으면 한글과 같은 2바이트 이상은 불러올 수 없기 때문에 보조스트림으로 감싸주어야한다.

- System.err
  - 표준 에러 출력(모니터) 스트림
  - ```System.err.println("에러 메세지");```

<details>
<summary>한 바이트 읽기</summary>

```java
package ch13;

import java.io.IOException;
import java.io.InputStreamReader;

public class SystemInTest {
    public static void main(String[] args) {

        System.out.println("알파벳 여러 개를 쓰고 [Enter]를 누르세요");

        int i;

        try {
            InputStreamReader irs = new InputStreamReader(System.in);
            while ((i = irs.read()) != '\n') {
                System.out.print((char)i);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
</details>

<details>
<summary>보조 스트림으로 감싸기</summary>

```java
package ch13;

import java.io.IOException;
import java.io.InputStreamReader;

public class SystemInTest {
    public static void main(String[] args) {

        System.out.println("알파벳 여러 개를 쓰고 [Enter]를 누르세요");

        int i;

        try {
            InputStreamReader irs = new InputStreamReader(System.in);
            while ((i = irs.read()) != '\n') {
                System.out.println((char)i);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
</details>

## 바이트 단위 입출력 스트림
### InputStream
- 바이트 단위 입력 스트림의 최상위 추상 클래스
- 다양한 추상 메서드들이 선언되어 있고, 하위 스트림 클래스가 상속받아서 구현한다.

#### 하위 클래스
- FileInputStream
  - 파일에서 바이트 단위로 자료를 읽는다.
- ByteArrayInputStream
  - 바이트 배열 메모리에서 바이트 단위로 자료를 읽는다.
- FilterInputStream
  - 기반 스트림에서 자료를 읽을 때 추가 기능을 제공하는 보조 스트림의 상위 클래스

#### 주요 메서드
- int read()
  - 입력 스트림으로부터 한 바이트의 자료를 읽고, 바이트 수를 반환
- int read(byte b[])
  - b 크기의 자료를 읽고, 바이트 수를 반환
- int read(byte b[], int off, int len)
  - b 크기 자료에서의 인덱스 0으로부터 Off만큼 떨어진 곳부터 len까지 자료를 읽고, 바이트 수를 반환
- void close()
  - 파일, 스트림을 불러왔을 때는 항상 close를 해주어야한다.

<details>
<summary>int read() 하나씩 출력하기</summary>

```java
package ch14;

import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamTest {
    public static void main(String[] args) {

        FileInputStream fis = null;
        try {
            fis = new FileInputStream("input.txt");
            System.out.println((char)fis.read());
            System.out.println((char)fis.read());
            System.out.println((char)fis.read());
        } catch (IOException e) {
            System.out.println(e);
            try {
                fis.close();
            } catch (IOException ioException) {
                System.out.println(ioException);
            } catch (Exception e2) {
                System.out.println(e2);
            }
        } finally {
            try {
                fis.close();
            } catch (IOException e) {
                System.out.println(e);
            } catch (Exception e2) {
                System.out.println(e2);
            }
        }
        System.out.println("end");
    }
}
```
</details>

<details>
<summary>파일 내 데이터 모두 불러오기</summary>

```java
package ch14;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FileTest {
    public static void main(String[] args) {
        // try의 resource에 파일을 넣어주면 AutoClosable이 되어 close를 하지 않아도 된다.
        int i ;
        try(FileInputStream fis = new FileInputStream("input.txt")){
            // read()메서드는 파일의 끝에서 -1을 반환해준다.
            while ( (i = fis.read()) != -1 ){
                System.out.print((char)i);
            }
        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
```
</details>

<details>
<summary>int read(바이트 배열) 출력하기</summary>

```java
package ch14;

import java.io.FileInputStream;
import java.io.IOException;

public class FileTest2 {

    public static void main(String[] args) {
        int i;
        try(FileInputStream fis = new FileInputStream("input2.txt");){

            byte[] bs = new byte[10];
            while ((i = fis.read(bs)) != -1) {
                for (int j = 0 ; j < i ; j++) {
                    System.out.print((char)bs[j]);
                }
                System.out.println(" : " + i + "바이트 읽음");
            }

        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
```
</details>

### OutputStream
- 바이트 단위 출력 스트림의 최상위 클래스
- InputStream과 같이 하위 스트림이 상속받아 구현한다.

#### 하위 클래스
- FileOutputStream
  - 파일에서 바이트 단위로 자료를 쓴다.
  > 파일을 불러올 때 해당 이름의 파일이 없으면 생성한다.
  > - default : Overwrite(덮어쓰기, 기존 데이터는 무시한다.)
  > - append를 true로 설정하여 이어서 작성할 수 있다.
      >   - ```new FileOutputStrem("a.txt", true);```

- ByteArrayOutputStream
  - byte배열에서 바이트 단위로 자료를 쓴다.
- FilterOutputStream
  - 기반 스트림에서 자료를 쓸 때 추가 기능을 제공하는 보조 스트림의 상위 클래스

#### 주요 메서드
- int write()
  - 한 바이트를 출력한다.
- int write(byte b[])
  - b[] 크기의 자료를 출력한다.
- int write(byte b[], int off, int len)
  - b 크기 자료에서의 인덱스 0으로부터 Off만큼 떨어진 곳부터 len까지 자료를 출력
- int flush()
  - 네트워크에서 socket을 쓰면 socket의 출력용 버퍼테 일정 크기의 데이터가 쌓이면 전송이된다.
  - 강제적으로 버퍼를 비워 자료를 출력하게한다.
- void close()
  - 리소스를 닫으면서 flush()를 수행한다.

<details>
<summary>Byte별로 write하기</summary>

```java
package ch14;

import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputTest {
    public static void main(String[] args) {

        try (FileOutputStream fos = new FileOutputStream("output.txt")){
            fos.write(65);
            fos.write(66);
            fos.write(67);
        } catch (IOException e){
            System.out.println(e);
        }
        System.out.println("end");
    }
}
```
</details>

<details>
<summary>Byte 배열 write하기</summary>

```java
package ch14;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputTest2 {
    public static void main(String[] args) throws FileNotFoundException {

        FileOutputStream fos = new FileOutputStream("output2.txt");

        try(fos){ //java9 이후로 제공
            byte[] bs = new byte[26];
            byte data = 65; // ASCII 'A'값
            for (int j = 0 ; j < bs.length ; j++){
                bs[j] = data;
                data++;
            }

            fos.write(bs);
        }catch(IOException e) {
            System.out.println(e);
        }
        System.out.println("완료되었습니다.");
    }
}
```
</details>

## 문자단위 입출력 스트림
### Reader
- 문자 단위 입력 스트림의 최상위 추상 클래스
- 추상 메서드를 하위 스트림이 상속받아 구현한다.

#### 하위 클래스
- FileReader
  - 파일에서 문자 단위로 읽는 스트림 클래스
- InputStreamReader
  - 보조 스트림
  - 바이트 단위로 읽은 자료(스트림)를 문자로 변환
- BufferedReader
  - 보조 스트림
  - 배열을 제공하여 한꺼번에 읽을 수 있는 기능을 제공

#### 주요 메서드
- 문자, 문자열을 읽어 온다.
- int read()
- int read(char[] buf)
- int read(char[] buf, int off, int len)
- void close()

<details>
<summary>예제 코드 확인하기</summary>

```java
package Online;

import java.io.FileReader;
import java.io.IOException;

public class IOTest {
    public static void main(String[] args) {

        try(FileReader fr = new FileReader("reader.txt")){
            int i;
            while ((i = fr.read()) != -1) {
                System.out.print((char) i);
            }

        } catch (IOException e) {

        }
    }
}
```
</details>

### Writer
- 문자 단위 출력 스트림의 최상위 클래스
- 추상 메서드를 하위 스트림이 상속받아 구현한다.

#### 하위 클래스
- FileWriter
- OutputStreamWriter
- Buffered Writer

#### 주요 메서드
- int write(int c)
- int write(char[] buf)
- int write(char[] buf, int off, int len)
- int write(String str)
- int write(String str, int off, int len)
- int flush()
- void close()

<details>
<summary>예제 코드 확인하기</summary>

```java
package Online;

import java.io.FileWriter;
import java.io.IOException;

public class WriterTest {
    public static void main(String[] args) {

        try (FileWriter fw = new FileWriter("writer.txt")) {
            fw.write('A');
            char buf[] = {'B', 'C', 'D', 'E', 'F'};
            fw.write(buf);
            fw.write("안녕하세요.");
            fw.write(buf, 1, 2);
            fw.write("65");

        }catch (IOException e) {

        }
    }
}

```
</details>

## 보조 스트림
- 실제 읽고 쓰는 스트림이 아닌 보조 기능을 제공하는 스트림이다.
  - FileInputStream과 FileOutputStream의 하위 클래스
- 여러 기능을 조합하여 사용할 수 있는 Decorator Pattern으로 구현된다.
- 생성자의 매개 변수로 파일 이름이 아닌 기반 스트림, 보조 스트림을 갖는다.

### InputStreamReader, OutputStreamReader

<details>
<summary>예제코드 확인하기</summary>

```java
package Online;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;

public class InputStreamReaderTest {
    public static void main(String[] args) {

        try (InputStreamReader isr = new InputStreamReader(new FileInputStream("reader.txt"))) {
            int i;
            while( (i = isr.read()) != -1) {
                System.out.print((char)i);
            }
        } catch(IOException e) {

        }
    }
}

```
</details>

### BufferedInputStream, BufferedOutputStream

<details>
<summary>예제코드 확인하기</summary>

```java
package Online;

import java.io.*;

public class CopyTest {
    public static void main(String[] args) {

        long millisecond = 0;

        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("reader.txt"));
             BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copy.txt"))) {

            millisecond = System.currentTimeMillis();

            int i;
            while ((i = bis.read()) != -1) {
                bos.write(i);
            }

            millisecond = System.currentTimeMillis() - millisecond;

        } catch (IOException e) {
            e.printStackTrace();
        }
        System.out.println(millisecond + "소요되었습니다.");
    }
}
```
</details>

### DataInputStream, DataOutputStream
- 자료가 메모리에 저장된 상태 그대로 읽거나 쓰는 스트림
  - 자료형, 크기를 유지하여 읽고 쓴다.
- 기록한 자료형 그대로 불러와야한다.

<details>
<summary></summary>

```java
package Online;

import java.io.*;

public class DataIOTest {
    public static void main(String[] args) {
        try (FileOutputStream fos = new FileOutputStream("data.txt");
             DataOutputStream dos = new DataOutputStream(fos)) {

            dos.writeByte(100);
            dos.writeChar('A');
            dos.writeInt(10);
            dos.writeFloat(3.14f);
            dos.writeUTF("Test");
        } catch (IOException e) {
            e.printStackTrace();
        }

        try (FileInputStream fis = new FileInputStream("data.txt");
             DataInputStream dis = new DataInputStream(fis))
        {
            System.out.println(dis.readByte());
            System.out.println(dis.readChar());
            System.out.println(dis.readInt());
            System.out.println(dis.readFloat());
            System.out.println(dis.readUTF());

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
</details>