# 컬렉션 프레임워크
## 특징
- 프로그램 구현에 필요한 자료구조를 구현해 놓은 JDK 라이브러리
- ```java.util``` 패키지에 구현되어 있다.

## 종류
### Collection 인터페이스
- 하나의 요소를 관리할 때 사용한다.
- 하위에 List와 Set 인터페이스가 있다.
#### Iterator
- 컬렉션 프레임워크에 저장된 요소를 하나씩 차례로 참조한다.
- List 인터페이스 : 순서가 정해져 있으므로 get(I)를 사용하여 참조 또한 가능하다.
- Set 인터페이스 : get(I) 메서드가 제공되지 않으므로 Iterator를 활용하여 객체를 순회한다.
- Iterator 함수를 호출하면 iterator가 반환이 된다.
> ```boolean hasNext()``` : 이후에 요소가 더 있는지를 체크하는 메서드
> ```E.next()``` : 다음에 있는 요소를 반환
<details>
<summary>구현 코드 확인하기</summary>

```java
Iterator<Member> ir = arrayList.iterator();
while (ir.hasNext()){
    Member member = ir.next();
    int tempId = member.getMemberId();
    if (tempId == memberId){
        arrayList.remove(member);
        return true;
    }
}
System.out.println(memberId + "가 존재하지 않습니다.");
return false;


```
</details>

#### List
- 객체를 순서에 따라 저장 및 관리할 때 필요한 메서드가 선언된 인터페이스
- 리스트 자료구조의 구현을 위한 인터페이스
- 객체의 중복을 허용한다.
- ArrayList, Vector, Queue, Stack, LinkedList 등

##### ArrayList
<details>
<summary>구현 코드 확인하기</summary>

```java
package ch10;

import java.util.ArrayList;

public class MemberArrayList {

    private ArrayList<Member> arrayList;

    public MemberArrayList(){
        arrayList = new ArrayList<>();
    }

    public MemberArrayList(int size){
        arrayList = new ArrayList<>(size);
    }

    public void addMember(Member member){
        arrayList.add(member);
    }

    public boolean removeMember(int memberId){
        // 중복이 가능하므로 어떤 요소를 삭제할 것인지 먼저 retrieve
        for (int i = 0; i < arrayList.size(); i++){
            Member member = arrayList.get(i);

            int tempId = member.getMemberId();
            if (tempId == memberId){
                arrayList.remove(i);
                return true;
            }
        }
        System.out.println(memberId + "가 존재하지 않습니다.");
        return false;
    }

    public void showAllMember(){

        for (Member i :arrayList){
            System.out.println(i);
        }
        System.out.println();
    }
}


```
</details>

#####

#### Set
- 아이디, 주민번호, 사번 등 유일한 값들의 집합을 관리할 때 사용한다.
- 저정된 순서와 출력 순서가 달라질 수 있다.

##### HashSet
- 검색을 위한 알고리즘인 Hash 방식으로 구성되어 있고, key
- 순서와 관계가 없다.
- 멤버의 중복 여부를 체크하기 위해 인스턴스의 동일성을 확인해야 한다.
    - 동일성 구현을 위해 필요에 따라 ```equals()```와 ```hashCode()``` 메서드를 재정의함
    - hashCode는 객체를 구분하기 위한 unique한 값을 return하게 해준다.
<details>
<summary> Override 코드 확인하기 </summary>

```java
@Override
    public int hashCode() {
        return memberId;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Member) {
            Member member = (Member)obj;
            if (this.memberId == member.memberId) {
                return true;
            }
            else return false;
        }
        return false;
    }

```
</details>

##### TreeSet
- 객체의 정렬에 사용하는 클래스 (중복을 허용하지 않는다.)
- 내부적으로 Binary Search Tree가 구현이 되어있다. (In-order traversal)
- Java에서는 balance를 위해 레드-블랙 트리를 사용한다.
- 비교하기 위한 요소를 구현하여야한다.
  - 작은 값은 왼쪽으로, 큰 값은 오른쪽으로 정렬하기 위해
  - element를 추가할 때마다 어떻게 비교할 지 ```comparable```, ```comparator```인터페이스를 구현하여야한다.
<details>
<summary>Comparable 코드 확인하기</summary>

```java
// Comparable 인터페이스를 implement하여 compareTo 메서드를 오버라이딩 
public class Member implements Comparable<Member> {
    ...
  @Override
  public int compareTo(Member member) {
    // this = 삽입되는 값
    // this가 크면 양수를 반환하여 오른쪽으로 이동하게 구현되어 있다.
    // 값이 같으면 (중복되면) 0을 반환하여 삽입되지 않는다.
    // 내림 차순은 양수, 음수를 바꿔주면 된다.
    return (this.memberId - member.memberId);
  }
}
```
</details>

<details>
<summary> Comparator 코드 확인하기 </summary>

```java
// 1. Comparator 인터페이스를 implement하여 compare 메서드 오버라이딩.
public class Member implements Comparator<Member> {
    ...

  @Override
  public int compare(Member o1, Member o2) {
    return (o1.memberId - o2.memberId);
  }
}

// 2. Comparator를 사용할 때
// TreeSet의 constructor(생성자)에 comparator를 구현한 객체를 지정해주어야한다.
public MemberTreeSet(){
        treeSet = new TreeSet<Member>(new Member());
}

// 3. 객체에 default constructor가 있어야 사용할 수 있다.
public Member(){}



```
</details>

##### Comparator 활용하기
- 이미 Comparable이 구현된 경우 Comparator로 비교 로직을 다시 구현할 수 있음
- String이 제공하는 compare 메서드는 오름차순으로 정렬이 된다.
- return값에 -1을 곱하여 내림차순으로 수정한다.
<details>
<summary>기존 코드</summary>

```java
TreeSet<String> set = new TreeSet<>();
        set.add("Park");
        set.add("Kim");
        set.add("Lee");

        System.out.println(set);
        
        // 결과 : [Kim, Lee, Park]
```
</details>

<details>
<summary>Comparator 활용 코드</summary>

```java
// 기존 String 클래스의 
class MyCompare implements Comparator<String>{

    @Override
    public int compare(String o1, String o2) {

        return o1.compareTo(o2)*-1;
    }
}

// TreeSet을 생성할 때 매개변수로 Comparator를 정의한 클래스를 넣어준다.
TreeSet<String> set = new TreeSet<>(new MyCompare());
      set.add("Park");
              set.add("Kim");
              set.add("Lee");

              System.out.println(set);
```
</details>



### Map
- Key와 value의 pair를 관리할 때 사용한다.
- Key값은 유일하여, 중복이 허용되지 않는다.

#### HashMap
- Key를 이용하여 값을 저장하고, 값을 꺼내온다.
- key가 되는 객체는 중복될 수 없고, 객체의 유일성을 비교하기 위해 ```equals()```, ```hashCode()``` 메서드를 구현하여야 한다. 
- Key는 중복이 될 수 없으므로 Set과 같은 개념
- Value는 중복이 될 수 있으므로 Collection과 같은 개념으로 보면 된다.
> 다른 키값이라도 hash함수를 통해 같은 index가 도출될 수 있다. 
> - Collision이 발생하며 오버헤드가 발생할 수 있다.
> - Java에서는 hash table의 Load Balance를 약 75%로 산정하여, 이를 방지한다.
>   - 100개가 들어갈 수 있는 테이블에 약 75개의 데이터를 수용함

<details>
<summary>예제 코드 확인하기</summary>

```java
 HashMap<Integer, String> hashMap = new HashMap<>();
        hashMap.put(1001, "Kim");
        hashMap.put(1002, "Lee");
        hashMap.put(1003, "Park");
        hashMap.put(1004, "Hong");

        System.out.println(hashMap);
```
</details>

<details>
<summary>새로운 객체 HashMap 생성</summary>

```java
import java.util.HashMap;
import java.util.Iterator;

public class MemberHashMap {

    private HashMap<Integer, Member> hashMap;

    public MemberHashMap(){
        hashMap = new HashMap<>();
    }

    public void addMember(Member member){
        hashMap.put(member.getMemberId(), member);
    }

    public boolean removeMember(int memberId){
        if (hashMap.containsKey(memberId)){
            hashMap.remove(memberId);
        }

        System.out.println("no element");
        return false;
    }

    public void showAll(){
        Iterator<Integer> ir = hashMap.keySet().iterator();

        while (ir.hasNext()){
            int key = ir.next();
            Member member = hashMap.get(key);
            System.out.println(member);
        }
    }

}


```
</details>

#### TreeMap
- 키로 정렬이 되고,  tree를 사용한다.
- TreeSet과 HashMap의 개념을 합한 클래스
- key, value 페어 key값을 기준으로 정렬
  - ```key```에 해당되는 class에 ```comparable```, ```comparator``` 정렬
  - key가 Integer, String이면 기존 메서드 활용하고, 아니면 오버라이딩하여 사용한다.
