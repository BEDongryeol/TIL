# 쓰레드

## 개념
### 프로세스 (Process)
- 실행 중인 프로그램이 실행되면 OS로부터 메모리를 할당받아 프로세스 상태가 된다.
- 프로그램이 메모리에 올라간 상태
- 프로세스는 하나 이상의 쓰레드를 가지게 된다.

### 쓰레드 (Thread)
- 프로세스가 메모리를 점유하고 CPU에서 수행이 된다.
- 쓰레드 : CPU에서 프로세스가 실행되는 단위
- 스케줄러 : 쓰레드의 배분
- 웹서버에 멀티쓰레드가 구현되어 있기 때문에 웹프로그래밍에서는 멀티쓰레드를 구현할 일이 없다.
- 쓰레드는 자신만의 작업 공간인 context(변수 정보 등)를 가지고 있다.

#### 쓰레드 상태 (Thread Status)
- Runnable
    - 쓰레드가 start 되면 쓰레드 풀에서 대기하게 된다.
    - 언제든지 CPU가 배분되면 실행될 수 있는 상태

- Not Runnable
    - CPU를 절대 점유할 수 없는 상태
    - 자바에서 메서드를 통해 Not Runnable로 만들 수 있다.
        1. **sleep(millisecond)**
            - millisecond후에 Runnable로 변경
        2. **wait()**
            - 자원이 한정되어 있을 때, 유효한 thread가 생길 때 까지 대기상태
            - 유효한 상태가 되면 **notify**, **notifyAll**을 통해 쓰레드를 불러온다.
        3. **join()**
            - 한 개의 쓰레드가 다른 쓰레드를 참조할 때 join()을 하면 참조하는 쓰레드가 끝날 때 까지 자기 자신을 Not Runnable 상태로 만든다.

- Dead
    - 쓰레드가 다 수행된 상태, 종료된 상태

#### wait() / notify()
- 리소스가 유효하지 않은 경우 유효할 때 까지 Thread는 wait상태가 된다.
- 유효한 리소스가 생기면 ```notify()```/```notifyAll()```를 통해 Thread가 호출된다.
    - notify() : 무작위 쓰레드 호출
    - notifyAll() : wait상태의 Thread 모두 호출
        - 유효한 리소스만큼 호출되며, 자원을 갖지 못하면 다시 wait상태가 된다.

- 예제
    - 도서관(공유 자원)에서 책을 빌리려하는 학생(Thread)이 있다.
    - 책(자원)은 한정되어 있고, 학생(쓰레드)들이 책(자원)을 빌리려 할 때 책이 없으면 wait()
    - 책(자원)이 반납되면 다른 학생(쓰레드)가 빌림

<details>
<summary>notify 예제 코드 확인하기</summary>

```java
package ch17;

import java.util.ArrayList;
import java.util.InputMismatchException;

class Library {
    public ArrayList<String> shelf = new ArrayList<>();

    public Library() {
        shelf.add("태백산맥1");
        shelf.add("태백산맥2");
        shelf.add("태백산맥3");
    }

    public synchronized String lendBook() throws InterruptedException {

        Thread t = Thread.currentThread();

        if (shelf.size() == 0) {
            System.out.println(t.getName() + " : waiting start");
            wait();
            System.out.println(t.getName() + " : waiting end");
        }

        if (shelf.size() > 0 ){
            String book = shelf.remove(0);
            System.out.println(t.getName() + " : " + book + " lend");
            return book;
        } else return null;
    }

    public synchronized void returnBook(String book) {
        Thread t = Thread.currentThread();
        shelf.add(book);
        System.out.println(t.getName() + " : " + book + " return");
        notify();
    }
}

class Student extends Thread{

    public Student(String name) {
        super(name);
    }

    @Override
    public void run() {
        try{
            String title = LibraryMain.library.lendBook();
            if (title == null) {
                System.out.println(getName() + " : 빌리지 못했음");
                return;
            }
            sleep(5000);
            LibraryMain.library.returnBook(title);
        } catch (InterruptedException e) {
            System.out.println(e);
        }
    }
}

public class LibraryMain {
    public static Library library = new Library();

    public static void main(String[] args) {

        Student std1 = new Student("std1");
        Student std2 = new Student("std2");
        Student std3 = new Student("std3");
        Student std4 = new Student("std4");
        Student std5 = new Student("std5");
        Student std6 = new Student("std6");

        std1.start();
        std2.start();
        std3.start();
        std4.start();
        std5.start();
        std6.start();
    }
}
```
</details>

<details>
<summary>notifyAll() 예제코드 확인하기</summary>

```java
package ch17;

import java.util.ArrayList;
import java.util.InputMismatchException;

class Library {
    public ArrayList<String> shelf = new ArrayList<>();

    public Library() {
        shelf.add("태백산맥1");
        shelf.add("태백산맥2");
        shelf.add("태백산맥3");
    }

    public synchronized String lendBook() throws InterruptedException {

        Thread t = Thread.currentThread();

        while (shelf.size() == 0) {
            System.out.println(t.getName() + " : waiting start");
            wait();
            System.out.println(t.getName() + " : waiting end");
        }

        if (shelf.size() > 0 ){
            String book = shelf.remove(0);
            System.out.println(t.getName() + " : " + book + " lend");
            return book;
        } else return null;
    }

    public synchronized void returnBook(String book) {
        Thread t = Thread.currentThread();
        shelf.add(book);
        System.out.println(t.getName() + " : " + book + " return");
        notifyAll();
    }
}

class Student extends Thread{

    public Student(String name) {
        super(name);
    }

    @Override
    public void run() {
        try{
            String title = LibraryMain.library.lendBook();
            if (title == null) {
                System.out.println(getName() + " : 빌리지 못했음");
                return;
            }
            sleep(5000);
            LibraryMain.library.returnBook(title);
        } catch (InterruptedException e) {
            System.out.println(e);
        }
    }
}

public class LibraryMain {
    public static Library library = new Library();

    public static void main(String[] args) {

        Student std1 = new Student("std1");
        Student std2 = new Student("std2");
        Student std3 = new Student("std3");
        Student std4 = new Student("std4");
        Student std5 = new Student("std5");
        Student std6 = new Student("std6");

        std1.start();
        std2.start();
        std3.start();
        std4.start();
        std5.start();
        std6.start();
    }
}
```
</details>

### 멀티 쓰레딩 (Multi-Threading)
- 여러 thread가 동시에 수행되는 프로그래밍, 여러 작업이 동시에 실행되는 효과
- thread는 각각 자신만의 작업 공간을 가짐 (context)
- 각 thread 사이에서 공유하는 자원이 있을 수 있음 (자바에서는 static instance)
- 여러 thread가 자원을 공유하여 작업이 수행되는 경우 서로 자원을 차지하려는 ```race condition```이 발생할 수 있음
- 이렇게 여러 thread가 공유하는 자원 중 경쟁이 발생하는 부분을 ```critical section```이라고 함
- critical section에 대한 ```동기화(일종의 순차적 수행)```를 구현하지 않으면 오류가 발생할 수 있음
    - 관련 : Synchronization, Monitor, Semaphore 등

### 동기화 (Synchronization)
- 공유 자원을 사용할 때 한 쓰레드가 사용 중이면 Lock을 걸어서 다른 쓰레드의 접근을 막는다
    1. 메서드에 ```synchronized``` 키워드 입력
    2. synchronized 블록 생성 @쓰레드, 메서드
        - ```synchronized(참조형 수식) {}```

<details>
<summary>예제 코드1 확인하기</summary>

```java
package ch17;

class Bank{
    private int money = 10000;

    public synchronized void saveMoney(int save){
        int m = getMoney();

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        setMoney(m + save);
    }

    public synchronized void minusMoney(int minus){
        int m = getMoney();

        try {
            Thread.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        setMoney(m-minus);
    }

    public int getMoney() {
        return money;
    }

    public void setMoney(int money) {
        this.money = money;
    }
}

class Park extends Thread{
    @Override
    public void run() {
        System.out.println("start save");
        SyncMain.myBank.saveMoney(3000);
        System.out.println("saveMoney : " + SyncMain.myBank.getMoney());
    }
}

class ParkWife extends Thread{
    @Override
    public void run() {
        System.out.println("start minus");
        SyncMain.myBank.minusMoney(1000);
        System.out.println("minusMoney : " + SyncMain.myBank.getMoney());
    }
}

public class SyncMain {
    public static Bank myBank = new Bank();

    public static void main(String[] args) {
        Park p = new Park();
        p.start();

        try {
            Thread.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        ParkWife pw = new ParkWife();
        pw.start();

    }
}
```
</details>

<details>
<summary>예제 코드 2 확인하기</summary>

```java
    public void saveMoney(int save){
        synchronized (this) {
            int m = getMoney();

            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            setMoney(m + save);
        }
    }
```
</details>

## 메서드
### 우선순위
- ```Thread.MIN_PRIORITY(=1) ~ Thread.MAX_PRIORITY(=10)```
- 디폴트 우선순위 : Thread.NORMAL_PRIORITY(=5)
- 우선 순위가 높은 Thread가 CPU의 배분을 받을 **확률이 높다**
- 우선순위 설정 및 호출이 가능하다
    - ```setPriority()/getPriority()```

<details>
<summary>예제 코드 확인하기</summary>

```java
class PriorityThread extends Thread{
	
	public void run(){
	
		int sum = 0;
		
		Thread t = Thread.currentThread();
		System.out.println( t + "start");
		
		for(int i =0; i<=1000000; i++){
			sum += i;
		}
		System.out.println( t.getPriority() + "end");
	}
}

public class PriorityTest {

	public static void main(String[] args) {

		int i;
		for(i=Thread.MIN_PRIORITY; i<= Thread.MAX_PRIORITY; i++){
			
			PriorityThread pt = new PriorityThread();
			pt.setPriority(i);
			pt.start();
		}
	}
}
```
</details>

### join
- 동시에 두 개 이상의 Thread가 실행 될 때 다른 Thread의 결과를 참조 하여 실행해야 하는 경우 join() 함수를 사용
- join() 함수를 호출한 Thread가 not-runnable 상태가 감
- 다른 Thread의 수행이 끝나면 runnable 상태로 돌아옴

<details>
<summary>예제 코드 확인하기</summary>

```java
package ch17;


public class JoinTest extends Thread{

    int start;
    int end;
    int total;

    public JoinTest(int start, int end) {
        this.start = start;
        this.end = end;
    }

    public void run() {
        int i;
        for (i = start; i <= end; i++){
            total += i;
        }
    }

    public static void main(String[] args) {

        JoinTest jt1 = new JoinTest(1, 50);
        JoinTest jt2 = new JoinTest(51, 100);

        jt1.start();
        jt2.start();
        // jt1, jt2가 끝날 때까지 main thread는 Not Runnable 상태가 된다.
        // 무한 루프 등 hang을 해결하기 위해 Exception을 날려준다.
        try {
            jt1.join();
            jt2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }


        // 아직 계산하고 있는 중이라서 값이 할당되지 않을 수 있다.
        int lastTotal = jt1.total + jt2.total;

        System.out.println("jt1.total = " + jt1.total);
        System.out.println("jt2.total = " + jt2.total);
        System.out.println("LastTotal = " + lastTotal);
    }
}
```
</details>

### interrupt
- 다른 Thread에 예외를 발생시키는 interrupt를 보낸다.
- join(), sleep(), wait()에 의해 Not Runnable 상태일 때, 호출하면 다시 runnable 상태가 된다.

### 쓰레드 종료
- 무한 반복의 경우 while(flag)의 flag를 false로 바꾸어 종료시킨다.
- while(true)가 아닌 다른 값을 입력해주기 위해 변수를 사용한다.

<details>
<summary>예제 코드 확인하기</summary>

```java
public class TerminateThread extends Thread{

	private boolean flag = false;
	int i;
	
	public TerminateThread(String name){
		super(name);
	}
	
	public void run(){
		
		
		while(!flag){
			try {
				sleep(100);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		
		System.out.println( getName() + " end" );
		
	}
	
	public void setFlag(boolean flag){
		this.flag = flag;
	}
	
	
	public static void main(String[] args) throws IOException {

		TerminateThread threadA = new TerminateThread("A");
		TerminateThread threadB = new TerminateThread("B");
		TerminateThread threadC = new TerminateThread("C");
		
		threadA.start();
		threadB.start();
		threadC.start();
		
		int in;
		while(true){
			in = System.in.read();
			if ( in == 'A'){
				threadA.setFlag(true);
			}else if(in == 'B'){
				threadB.setFlag(true);
			}else if( in == 'C'){
				threadC.setFlag(true);
			}else if( in == 'M'){
				threadA.setFlag(true);
				threadB.setFlag(true);
				threadC.setFlag(true);
				break;
			}else{
				System.out.println("type again");
			}
		}
		
		System.out.println("main end");
		
	}
}
```
</details>

## 구현
### extends Thread
- Thread가 수행이되면 run 메서드가 실행된다.
- 기본적으로 메인 쓰레드가 수행이된다.
- 예제에서 쓰레드 2개를 추가하여 테스트 (메인 포함 총 3개 쓰레드)

<details>
<summary>예제 코드 확인하기</summary>

```java
package ch17;

class MyThread extends Thread {
    public void run() {
        int i;
        for (i = 1; i<=200; i++){
            System.out.print(i + "\t");
        }
    }
}

public class ThreadTest {
    public static void main(String[] args) {
        // 현재 main 내의 thread확인
        // [호출한 함수, priority, threadGroup]
        System.out.println(Thread.currentThread() + "start");
        MyThread th1 = new MyThread();
        MyThread th2 = new MyThread();

        th1.start();
        th2.start();
        System.out.println(Thread.currentThread() + "end");
    }
}
```
</details>

### implements Runnable
- ```run 메서드```를 override 해주어어야 한다.
- Main에서 Thread를 생성할 때 ```runnable 객체```를 생성하여 ```생성자의 parameter```로 넣어준다.

<details>
<summary>예제 코드 확인하기</summary>

```java
package ch17;

class MyThread implements Runnable {

    @Override
    public void run() {
        int i;
        for (i = 0; i <= 200; i++) {
            System.out.print(i + "\t");
        }
    }
}

public class ThreadTest {
    public static void main(String[] args) {

        System.out.println(Thread.currentThread() + "start");
        // runnable 객체 생성
        MyThread runnable = new MyThread();
        // runnable기반으로 쓰레드가 돌아간다.
        Thread th1 = new Thread(runnable);
        Thread th2 = new Thread(runnable);

        th1.start();
        th2.start();

        System.out.println(Thread.currentThread() + "end");
    }
}
```
</details>



    
    